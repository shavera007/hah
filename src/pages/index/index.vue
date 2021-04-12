<template>
  <div @click="clickHandle">
    <button class="btn" lang='zh_CN' open-type="getUserInfo" @getuserinfo="onGetUserInfo" withCredentials="true">微信登录</button>
    <div class="info">若您的手机号未注册，将为您自动注册，注册或登录即代表您同意《用户协议》>和《隐私政策》</div>
    <Modal v-if="showPhoneModal" title='授权提示' content='为了您的账号安全，需要授权手机号码才能完成登录' @close='showPhoneModal = false' @confirm='onGetphonenumber' ></Modal>
    <div class="userinfo" @click="bindViewTap">
      <img class="userinfo-avatar" v-if="userInfo.avatarUrl" :src="userInfo.avatarUrl" background-size="cover" />
      <img class="userinfo-avatar" src="/static/images/user.png" background-size="cover" />

      <div class="userinfo-nickname">
        <card :text="userInfo.nickName"></card>
      </div>
    </div>

    <div class="usermotto">
      <div class="user-motto">
        <card :text="motto"></card>
      </div>
    </div>

    <form class="form-container">
      <input type="text" class="form-control" :value="motto" placeholder="v-model" />
      <input type="text" class="form-control" v-model="motto" placeholder="v-model" />
      <input type="text" class="form-control" v-model.lazy="motto" placeholder="v-model.lazy" />
    </form>

    <a href="/pages/counter/main" class="counter">去往Vuex示例页面</a>

    <div class="all">
        <div class="left">
        </div>
        <div class="right">
        </div>
    </div>
  </div>
</template>

<script>
import card from '@/components/card'
import Modal from './components/modal'
export default {
  data () {
    return {
      motto: 'Hello miniprograme',
      userInfo: {
        nickName: 'mpvue',
        avatarUrl: ''
      },
      showPhoneModal: false,
      phoneData: null,
      userData: null,
    }
  },

  components: {
    card,
    Modal
  },

  methods: {
    onGetUserInfo (e) {
      console.log(e);
      if (e.mp.detail.encryptedData) {
        let data = {
          'encryptedData': e.mp.detail.encryptedData,
          'iv': e.mp.detail.iv
        }
        this.userData = data
        this.showPhoneModal = true
      }
    },
    onGetphonenumber (e) {
      if (e.mp.detail.encryptedData) {
        let data = {
          'encryptedData': e.mp.detail.encryptedData,
          'iv': e.mp.detail.iv
        }
        this.phoneData = data
        // wx.setStorage({
        //   key: 'registeData',
        //   data: data
        // })
        this.register()
      }
    },
    async register () {
      console.log("Register")
    },
    bindViewTap () {
      const url = '../logs/main'
      if (mpvuePlatform === 'wx') {
        mpvue.switchTab({ url })
      } else {
        mpvue.navigateTo({ url })
      }
    },
    clickHandle (ev) {
      console.log('clickHandle:', ev)
      // throw {message: 'custom test'}
    }
  },

  created () {
    // let app = getApp()
  }
}
</script>

<style scoped>
.userinfo {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.userinfo-avatar {
  width: 128rpx;
  height: 128rpx;
  margin: 20rpx;
  border-radius: 50%;
}

.userinfo-nickname {
  color: #aaa;
}

.usermotto {
  margin-top: 150px;
}

.form-control {
  display: block;
  padding: 0 12px;
  margin-bottom: 5px;
  border: 1px solid #ccc;
}
.all{
  width:7.5rem;
  height:1rem;
  background-color:blue;
}
.all:after{
  display:block;
  content:'';
  clear:both;
}
.left{
  float:left;
  width:3rem;
  height:1rem;
  background-color:red;
}

.right{
  float:left;
  width:4.5rem;
  height:1rem;
  background-color:green;
}
</style>
