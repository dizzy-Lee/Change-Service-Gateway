<template>
  <div>
    <div class="hello" v-for="(item, key, index) in this.$store.state.data" :key="index">
      <div class="card">
        <span class="card-user">{{Object.keys(item)[0]}}</span>
        <div v-show="show1" class="card-message1">
          <span class="card-message1-number">{{death}} / {{alive}}</span>
        </div>
        <div v-show="show2" class="card-message2">
          <span class="card-message2-number">{{death}}</span>
        </div>
        <div v-show="show3" class="card-message3">
          <span class="card-message3-number">{{item[Object.keys(item)[0]].server==null?alive = 0:alive = item[Object.keys(item)[0]].server.length}}</span>
        </div>
        <p class="card-p">{{item[Object.keys(item)[0]].describe}}</p>
        <div class="card-hr"></div>
        <button @click="clickDelete(Object.keys(item)[0])" class="card-delete text-pointer">删除服务</button>
        <button @click="download(item)" class="card-download text-pointer">下载接口定义</button>
      </div>

      <!-- 遮罩层 -->
      <div v-show="newService" class="hide-message">
        <div class="hide-message-delete">
          <p class="hide-message-delete-p1">删除服务</p>
          <p class="hide-message-delete-p2">Delete Service</p>
          <div class="hide-message-delete-data">
            <p class="hide-message-delete-data-p1">User</p>
            <p class="hide-message-delete-data-p2">用户信息服务</p>
          </div>
          <button @click="clickConfirm" class="hide-message-y text-pointer">确认删除</button>
          <button @click="clickCancel" class="hide-message-n text-pointer">取消</button>
        </div>
      </div>

    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: "HelloWorld",
  data() {
    return {
      show1: false,
      show2: false,
      show3: false,
      user: "User",
      death: "",
      alive: "",
      newService: false,
      name: 'null'
    };
  },
  created: function () {
    if (this.death != "") {
      if (this.alive != "") {
        this.show1 = true
      } else {
        this.show2 = true
      }
    } else {
      this.show3 = true
    }
  },
  methods: {
    clickCancel() {
      this.newService = false;
    },
    clickDelete(item) {
      this.newService = true;
      this.name = item
    },
    clickConfirm() {
      this.clickCancel();
      // console.log(this.name)
      axios
        .post('/api/manage/remove', {name: this.name})
        .then(res => {
          if (res.data.status == true) {
            alert('删除成功')
            location.reload();
          } else {
            alert(res.data.result)
          }
          // console.log(res)
        })
    },
    download (item) {
      // console.log(item)
      window.open(this.apiAddress + item[Object.keys(item)[0]].proto)
    }
  }
};
</script>

<style lang="less" scoped>
@import "../assets/style/main.less";
.hello {
  .card {
    width: 342px;
    height: 143px;
    margin-right: 24px;
    margin-bottom: 24px;
    padding-top: 32px;
    // padding-left: 32px;
    background: #ffffff;
    box-shadow: 0 10px 40px 0 rgba(0, 0, 0, 0.1);
    border-radius: 10px;
    float: left;
    position: relative;
    .card-user {
      margin-top: 32px;
      margin-left: 32px;
      font-size: 24px;
      color: #212121;
      letter-spacing: 0;
      text-align: justify;
      line-height: 24px;
    }
    .card-message1 {
      float: right;
      width: 95px;
      height: 32px;
      margin-right: 32px;
      background: #f5a623;
      border-radius: 100px;
      text-align: center;
      position: relative;
      top: 8px;
      .card-message1-number {
        font-size: 16px;
        color: #ffffff;
        letter-spacing: 0;
        text-align: justify;
        line-height: 32px;
      }
    }
    .card-message2 {
      float: right;
      width: 67px;
      height: 32px;
      margin-right: 32px;
      background: #e91e63;
      border-radius: 100px;
      text-align: center;
      position: relative;
      top: 8px;
      .card-message2-number {
        font-size: 16px;
        color: #ffffff;
        letter-spacing: 0;
        text-align: justify;
        line-height: 32px;
      }
    }
    .card-message3 {
      float: right;
      width: 42px;
      height: 32px;
      margin-right: 32px;
      background: #03a9f4;
      border-radius: 100px;
      text-align: center;
      position: relative;
      top: 8px;
      .card-message3-number {
        font-size: 16px;
        color: #ffffff;
        letter-spacing: 0;
        text-align: justify;
        line-height: 32px;
      }
    }
    .card-p {
      width: 180px;
      color: #9e9e9e;
      letter-spacing: 0;
      text-align: justify;
      line-height: 16px;
      position: absolute;
      left: 32px;
      top: 46px;
      overflow: hidden;
      text-overflow: ellipsis;
      white-space: nowrap;
    }
    .card-hr {
      width: 278px;
      height: 1px;
      background: #eee;
      position: relative;
      top: 48px;
      left: 32px;
      border: 0;
    }
    .card-delete {
      width: 134px;
      position: absolute;
      top: 121px;
      left: 32px;
      outline: 0 none !important;
    }
    .card-download {
      width: 128px;
      position: absolute;
      top: 121px;
      left: 182px;
      outline: 0 none !important;
    }
  }
  .hide-message {
    width: 100%;
    height: 100%;
    background: rgba(255,255,255,0.3);
    position: absolute;
    top: 0;
    left: 0;
    z-index: 888;
    .hide-message-delete {
      width: 310px;
      height: 246px;
      padding-top: 32px;
      padding-left: 32px;
      margin: 0 auto;
      margin-top: 261px;
      background: #ffffff;
      box-shadow: 0 10px 40px 0 rgba(0, 0, 0, 0.1);
      border-radius: 10px;
      .hide-message-delete-p1 {
        margin: 0;
        font-size: 24px;
        color: #212121;
        letter-spacing: 0;
        text-align: justify;
        line-height: 24px;
        font-weight: 600;
      }
      .hide-message-delete-p2 {
        margin-top: 8px;
        margin-bottom: 32px;
        font-size: 12px;
        color: #9e9e9e;
        letter-spacing: 0;
        text-align: justify;
        line-height: 12px;
      }
      .hide-message-delete-data {
        width: 278px;
        height: 68px;
        margin-bottom: 32px;
        border: 1px solid #eee;
        border-radius: 10px;
        text-align: center;
        .hide-message-delete-data-p1 {
          font-weight: 600;
          margin-bottom: 4px;
        }
        .hide-message-delete-data-p2 {
          margin-top: 0;
          font-size: 12px;
          color: #9e9e9e;
        }
      }
      .hide-message-y {
        width: 131px;
        outline: 0 none !important;
      }
      .hide-message-n {
        width: 132px;
        margin-left: 16px;
        color: #fff;
        background: #03a9f4;
        outline: 0 none !important;
      }
    }
  }
}
</style>
