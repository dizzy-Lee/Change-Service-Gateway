<template>
  <div id="app">
    <div class="body">
      <div class="header">
        <span class="header-title">Change Service Gateway</span>
        <button @click="clickNewService" class="header-new text-pointer">新建服务</button>
        <span class="header-line"></span>
        <span class="header-name">{{name}}</span>
        <img class="header-avatar" :src="image">
        <div class="header-stitle">
          启嘉网微服务网关层
        </div>
      </div>
      <router-view/>
    </div>

    <!-- 遮罩层 -->
    <div @click="closeConfirmService" v-show="newService" class="hide-message">
      <div class="hide-message-create" @click="stopPropagation">
        <p class="hide-message-create-p1">新建服务</p>
        <p class="hide-message-create-p2">Create New Service</p>
        <input v-model="serviceName" class="hide-message-create-input" placeholder="请输入服务名称">
        <input v-model="serviceDescribe" class="hide-message-create-input" placeholder="请输入服务描述">
        <div class="hide-message-crete-file"><p style="line-height: 30%;">请拖动文件到此处</p></div>
        <button @click="clickConfirmService" class="hide-message-confirm text-pointer">提交</button>
        <input type="file" @change="getFile" 
          style="
            position: absolute;
            top: 216px;
            left: 32px;
            width: 273px;
            height: 38px;
            opacity: 0;
          "
        >
      </div>
    </div>

  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: "App",
  data() {
    return {
      serviceDescribe: '',
      serviceName: '',
      file: '',
      newService: false,
      name: "",
      image: ""
    };
  },
  mounted: function () {
    this.getUserInfo()
    this.getAllServiceInfo()
  },
  methods: {
    getUserInfo() {
      axios
        .get('/api/user/getuserinfo')
        .then(res => {
          this.name = res.data.result.username
          this.image = this.apiAddress + res.data.result.avatar
        })
    },
    getAllServiceInfo() {
      axios
        .get('/api/manage/GetAllServiceInfo')
        .then(res => {
          if (res.data.status == true) {
            this.$store.commit('changeData', res.data.result)
          } else {
            console.log(res.data.status)
          }
        })
    },
    closeConfirmService () {
      this.newService = false
    },
    stopPropagation () {
      event.stopPropagation()
    },
    clickNewService () {
      this.newService = true
    },
    clickConfirmService () {
      if (this.serviceName == '') {
        alert('服务名称不能为空！')
      } else if (this.serviceDescribe == '') {
        alert('服务描述不能为空！') 
      } else {
        this.newService = false
        this.submit()
      }
    },
    //上传表单
    getFile(event) {
      this.file = event.target.files[0]
      // console.log(this.file)
    },
    submit () {
      event.preventDefault(); //取消默认行为
      let formdata = new FormData();
      formdata.append("file", this.file);
      formdata.append("name", this.serviceName);
      formdata.append("describe", this.serviceDescribe);
      axios
        .post('/api/manage/register' ,formdata)
        .then(res => {
          if (res.data.status == false) {
            console.error('创建失败，' + res.data.result)
          }
          // console.log(res)
          this.getAllServiceInfo()
        })
        .catch(err => {
          console.log(err)
        })
    }
  },
};
</script>

<style lang="less" scoped>
@import "./assets/style/main.less";

#app {
  font-family: PingFangSC-Semibold;
}

input::-webkit-input-placeholder, textarea::-webkit-input-placeholder { 
 /* WebKit browsers */ 
color: #9e9e9e; 
} 
input:-moz-placeholder, textarea:-moz-placeholder { 
/* Mozilla Firefox 4 to 18 */ 
color: #9e9e9e; 
} 
input::-moz-placeholder, textarea::-moz-placeholder { 
 /* Mozilla Firefox 19+ */ 
color: #9e9e9e; 
} 
input:-ms-input-placeholder, textarea:-ms-input-placeholder { 
 /* Internet Explorer 10+ */ 
color: #9e9e9e; 
} 

.body {
  margin: 0 auto;
  width: 1200px;
  // background: red;
  .header {
    margin-top: 49px;
    .header-title {
      font-size: 32px;
      margin-right: 398px;
      font-weight: 500;
    }
    .header-new {
      width: 111px;
      outline: 0 none !important;
    }
    .header-line {
      height: 32px;
      border: 1px solid #eeeeee;
      margin-left: 32px;
    }
    .header-name {
      font-size: 16px;
      color: #212121;
      letter-spacing: -0px;
      text-align: justify;
      margin-left: 32px;
      margin-right: 32px;
    }
    .header-avatar {
      width: 48px;
      height: 48px;
      border-radius: 24px;
      position: relative;
      top: 17px;
    }
    .header-stitle {
      font-size: 16px;
      color: #9e9e9e;
      letter-spacing: 0;
      text-align: justify;
      line-height: 16px;
      margin-top: 8px;
      margin-bottom: 72px;
    }
  }
}
.hide-message {
  width: 100%;
  height: 100%;
  background: rgba(255,255,255,0.8);
  position: absolute;
  top: 0;
  left: 0;
  z-index: 888;
  .hide-message-create {
    position: relative;
    z-index: 999;
    width: 310px;
    height: 316px;
    padding-top: 32px;
    padding-left: 32px;
    margin: 0 auto;
    margin-top: 261px;
    background: #ffffff;
    box-shadow: 0 10px 40px 0 rgba(0, 0, 0, 0.1);
    border-radius: 10px;
    .hide-message-create-p1 {
      margin: 0;
      font-size: 24px;
      color: #212121;
      letter-spacing: 0;
      text-align: justify;
      line-height: 24px;
      font-weight: 600;
    }
    .hide-message-create-p2 {
      margin-top: 8px;
      margin-bottom: 32px;
      font-size: 12px;
      color: #9e9e9e;
      letter-spacing: 0;
      text-align: justify;
      line-height: 12px;
    }
    .hide-message-create-input {
      width: 257px;
      height: 38px;
      padding-left: 16px;
      margin-bottom: 16px;
      background: rgba(68, 68, 68, 0.07);
      border: none;
      border-radius: 5px;
      outline: 0 none !important;
      font-size: 16px;
      color: #9e9e9e;
      letter-spacing: 0;
    }
    .hide-message-crete-file {
      width: 255px;
      height: 36px;
      padding-left: 16px;
      margin-bottom: 24px;
      border: 1px solid rgba(68, 68, 68, 0.07);
      border-radius: 5px;
      font-size: 16px;
      color: #9e9e9e;
      letter-spacing: 0;
      -moz-user-select: -moz-none;
      -khtml-user-select: none;
      -webkit-user-select: none;
      -ms-user-select: none;
      user-select: none;
    }
    .hide-message-confirm {
      width: 273px;
      color: #fff;
      background: #03a9f4;
      outline: 0 none !important;
    }
  }
}
</style>
