<template>
    <div class="revise add">
      <group>
        <x-input placeholder="请输入账户姓名" v-model="realname">
          <img slot="label"   src="../../assets/img/my/zhxm.png">
        </x-input>
        <x-input placeholder="请输入银行名称" v-model="bank" >
          <img slot="label"   src="../../assets/img/my/zfbzh.png">
        </x-input>
        <x-input placeholder="请输入支行名称" v-model="subbranch" >
          <img slot="label"   src="../../assets/img/my/zfbzh.png">
        </x-input>
        <x-input placeholder="请输入银行卡号" v-model="subcard" >
          <img slot="label"   src="../../assets/img/my/zfbzh.png">
        </x-input>
        <x-input  class="weui-vcode" placeholder="请输入手机验证码" v-model="smsCode">
          <img slot="label"   src="../../assets/img/my/sjyzm.png">
          <x-button slot="right" type="primary" mini class="send" @click.native="handSmsCode" style="background: #01C2C7">{{getCode}}</x-button>
        </x-input>
      </group>
      <div class="primary_btn" @click="bind" style="background: #01C2C7">确定</div>
    </div>
</template>

<script>
import {Group, XInput, Cell, XHeader, Confirm, XButton} from 'vux'
export default {
  name: 'revise',
  components: {
    Group, Cell, XHeader, Confirm, XInput, XButton
  },
  data () {
    return {
      data: this.$store.state.user.userInfo,
      realname: '',
      alipayid: '',
      getCode: '获取验证码',
      timerFlag: false,
      smsCode: '',
      bank:'',
      subbranch:'',
      subcard:''
    }
  },
  mounted: function () {
    // 删除
  },
  methods: {
    handSmsCode () {
      // if (!/[0-9]{11}/.test(this.mobile)) {
      //   this.$vux.toast.text('请输入正确的手机号')
      //   return
      // }
      if (this.timerFlag === false) {
        this.$http.post('/amoy/auth/sms-verifycode', {
          type: 'update',
          mobile: this.data.mobile
        }, true).then(res => {
          this.timerFlag = !this.timerFlag
          this.$vux.toast.text(res.msg)
          this.countDown(60)
        })
      } else {
        return ''
      }
    },
    // 每秒执行
    countDown (time) {
      let count = time
      setTimeout(() => {
        count--
        if (time <= 0) {
          this.getCode = '获取验证码'
          this.timerFlag = false
        } else if (count !== time) {
          this.getCode = '重新获取' + count
          this.countDown(count)
        }
      }, 1000)
    },
    bind () {
      this.$http.post('/amoy/user/bind-card', {
        real_name: this.realname,
        bank:this.bank,
        sub_branch:this.subbranch,
        bank_card:this.subcard,
        smsCode: this.smsCode,
        alipayid: this.alipayid,
      }, true).then(res => {
        if (res.code === 0) {
          this.$vux.toast.text(res.msg)
          this.data.bank_card = this.subcard  // 把后端返回的数据 加到 本地对象里 ,把卡号 返回过来；
          this.$store.commit('setUserInfo', this.data)
          this.$router.go(-1)
        }
      })
    }
  }
}
</script>
<style lang="less">
  @import "../../assets/less/common";
  .revise{
    .weui-cell{
      padding: 10px 15px !important;
    }
    .weui-cells{
      margin-top: 0;
    }
    .weui-cell__hd{
      display: flex;
      align-items: center;
      img{
        margin-right: .2rem;
        width: .22rem;
        max-height: .36rem;
      }
    }
    .weui-input{
      font-size: .28rem;
    }
    .weui-cell{
      padding: .27rem .3rem;
    }
  }
  .add .weui-cells:after{
    display: none;
  }
</style>
<style scoped lang="less">
@import "../../assets/less/common";
.revise .weui-cell {
  margin: .3rem;
  background: #f4f4f4;
}
.revise .weui-cell:before{
  display: none;
}
</style>
