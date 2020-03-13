<template>
  <div id="wechat_login">
    <i class="iconfont icon-close" @click="goBack"></i>
    <div class="wechat_login_img">
      <img src="../../assets/img/balance/logo.png" alt="">
    </div>
    <div class="edit_button" >
      <button v-show="isAgree" class="btn_bright_red"  @click="jumpTo('/login')" style="background:#01C2C7">
        登录
      </button>
      <button v-show="!isAgree"  class="btn_bright_red"  style="background:gray" >
        登录
      </button>
    </div>
    <ul class="wechat_login_enroll">
      <li class="li1" style="color: #01C2C7"  @click="jumpTo('/register')">注册账号</li>
      <span style="color: #01C2C7" >|</span>
      <li class="li2" style="color: #01C2C7"  @click="jumpTo('/login')">手机登录</li>
    </ul>
    <p class="wechat_login_protocol" @click="switchAgree()" style="display:flex;align-items:center;text-align:center">  
      <img class="is-agree" v-show="!isAgree"  src="../../assets/img/balance/fxk.png" alt="">
      <img class="is-agree" v-show="isAgree"  src="../../assets/img/balance/afxk.png" alt="">
      登录注册即表示同意 <span @click="jumpTo('/about?title=用户使用协议&type=7')" style="color: #01C2C7">《用户使用协议》</span>
    </p>
  </div>
</template>

<script>
export default {
  name: 'wechatLogin',
  data () {
    return {
      wx: false,
      data: '',
      auth: {},
      auths: {},
      authBtns: ['weixin'],
      loading: false,
      isAgree:true,
      vuegConfig: {
       disable: false
      }
    }
  },
  inject: ['reload'],
  mounted: function () {
    let that = this
    if (api.systemType === 'ios') {
      that.wx = api.appInstalled({
        sync: true,
        appBundle: 'weixin://'
      })
    } else {
      that.wx = api.appInstalled({
        sync: true,
        appBundle: 'com.tencent.mm'
      })
    }
  },
  methods: {
    switchAgree(){
      this.isAgree = !this.isAgree
    },
    wechatLogin () {
      let that = this
      let wx = api.require('wx')
      wx.auth({
        apiKey: ''
      }, function (ret, err) {
        if (ret.status) {
          that.$http.post('/amoy/auth/we-login', {
            code: ret.code,
            registration_id: that.$store.state.global.registrationId
          }, false, true).then(res => {
            
            if (res.code === 0) {
              that.$store.commit('setToken', res.data.token)
              that.$store.commit('setUserInfo', res.data)
              that.$store.commit('setCouponpassMoney', res.data.couponpass_money)
              that.$router.push(that.$store.state.global.firstNav)
            } else if (res.code === 2) {
              that.jumpTo({name: 'wxbind', query: {type: 'new', token: res.data.access_token, openid: res.data.openid}})
            } else {
              that.$vux.toast.text(res.msg)
            }
          })
        } else {
          console.log(err.code)
        }
      })
    }
  }
}
</script>

<style scoped lang="less">
  #wechat_login{
    height: 100%;
    background: #fff;
    overflow-y: auto;
    .icon-close{
      position: absolute;
      right: .5rem;
      top: .7rem;
      font-size: .3rem;
    }
    .is-agree{
      width:0.35rem;
      height:0.35rem;
    }
  }
</style>
