<template>
  <div id="login">
    <x-header :left-options="{showBack: false}">
      <a slot="left" class="backBox" @click="goBack()">
        <i class="iconfont icon-back1"></i>
      </a>
      {{$route.meta.title}}
    </x-header>
    <div class="wechat_login_img_logo">
      <img src="../../assets/img/balance/logo.png" alt="">
    </div>
    <div class="input_list login_input">
      <div class="label1">
        <input type="tel" placeholder="请输入手机号" v-model="mobile">
      </div>
      <div class="label2">
        <input type="password" placeholder="请输入密码" v-model="password" @keyup.enter="login">
      </div>
    </div>
    <div class="edit_button btn_login">
      <button class="btn_bright_red" style="background: #01C2C7" @click="login">登录</button>
    </div>
    <div class="edit_button btn_register">
      <button class="btn_bright_red" @click="jumpTo('/register')" style="borderColor: #01C2C7">注册</button>
    </div>
    <div class="login_enroll_div">
      <ul class="login_enroll">
        <li class="li1" @click="jumpTo('/forget')" style="color: #01C2C7" >忘记密码?</li>
        <span style="color: #01C2C7" >|</span>
        <li class="li2" style="color: #01C2C7"  @click="jumpTo('/mobileLogin')">验证码登录</li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'wechat_login',
  data () {
    return {
      mobile: '',
      password: ''
    }
  },
  inject: ['reload'],
  methods: {
    login () {
      if (!/[0-9]{11}/.test(this.mobile)) {
        this.$vux.toast.text('请输入正确的手机号')
        return
      }
      let that = this
      this.$http.post('/amoy/auth/login', {
        mobile: this.mobile,
        password: this.password,
        registration_id: that.$store.state.global.registrationId
      }, true).then(res => {
       
        
        this.$store.commit('setToken', res.data.token)
        // this.$store.commit('setUserInfo', res.data)
        this.$store.commit('setUserInfo', res.data)
        this.$store.commit('setCouponpassMoney', res.data.couponpass_money)
        //this.reload()
        if(res.code==0) {
          this.$router.replace({
            path:'/home',
          })
        }
       
      })
    }
  },
  mounted: function () {
    // 删除
  }
}
</script>

<style scoped lang="less">
  #login{
    overflow-x: hidden;
    height: 100%;
    background: #fff;
    overflow-y: auto;
  }
  .vux-header{
    position: relative !important;
  }
  .wechat_login_img_logo{
    text-align:center;
  }
  .wechat_login_img_logo>img{
    width:80px;
    height:80px;
    margin:40px auto;
    margin-bottom:80px;
  }
</style>
