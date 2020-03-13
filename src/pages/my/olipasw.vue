<template>
    <div>
        <!-- 修改密码 -->
        <div class="namme">
          <ul>
            <li>
              <span>手机号</span>
              <input type="text" placeholder="请输入手机号" v-model="phone">
            </li>
            <li>
              <span>验证码</span>
              <input type="text" placeholder="请输入验证码" v-model="smsCode">
              <span id="acctt" @click="getcode()">{{title}}</span>
            </li>
            <li>
              <span>新密码</span>
              <input type="password" placeholder="请输入新密码" v-model="pass">
            </li>
            <li>
              <span>新密码</span>
              <input type="password" placeholder="请输入确认新密码" v-model="pass1">
            </li>
          </ul>
        </div>
        <div class="btnn"  @click="send">确认</div>
    </div>
</template>

<script>
export default {
  name: "forget",
  data() {
    return {
      phone: "", //手机号
      smsCode: "", //验证码
      pass: "", //密码
      pass1: "", //密码
      title: "获取验证码"
    };
  },
  methods: {
    change(val) {
      // 获取验证码倒计时
      if (val <= 0) {
        this.title = "获取验证码";
        this.flag = false;
      }
      setTimeout(() => {
        this.title = "重新获取" + val;
        this.change(--val);
      }, 1000);
    },
    getcode() {
      //获取验证码
      if (this.flag === true) return;
      this.flag = true;
      this.$http
        .post(
          "",
          {
            mobile: this.phone,
            scenario: "reset"
          },
          true
        )
        .then(res => {
          this.change(60);
        });
    },
    send() {
      //注册
      if (this.phone == "" || this.pass == "" || this.pass1 == "") {
        this.$vux.toast.text("所有都要填写不能为空");
        return;
      } else if (!/[0-9]{11}/.test(this.phone)) {
        this.$vux.toast.text("请输入正确的手机号");
        return;
      } else if (this.pass != this.pass1) {
        this.$vux.toast.text("两次密码输入不一致");
        return;
      } else {
        this.$http
          .post("/m1/user/set-user-pay-password", {
            // mobile: this.phone, 后台需要修改接口， 
            payPassword: this.pass,
            // smsCode: this.smsCode //短信验证码
          })
          .then(res => {
            if (res.code == 0) {
              this.$vux.toast.text(res.msg);
              // this.$router.push('/')
              this.jumpTo("/");
            }
          });
      }
    }
  }
};
</script>

<style scoped>
#acctt {
  font-size: .32rem;
  position: absolute;
  right: .2rem;
  top: .2rem;
  font-weight:500;
  color:rgba(255,62,85,1);
}
#acctt::after {
  content: "";
  position: absolute;
  left: -.25rem;
  top: .33rem;
  width: 1px;
  height: .4rem;
  background:rgba(210,210,210,1);
}
/* 竖线 */
#viphead {
    width: 100%;
    height: 100%;
    margin-bottom:0.01rem;
}
.heee {
    position: absolute;
    top: .27rem;
    left: .3rem;
}
/* 顶部 */
/* 顶部 */
.namme {
    width: 100%;
    font-size: .33rem;
    box-sizing: border-box;
    background-color: #fff;
}
.namme li {
    list-style: none;
    height: 1rem;
    line-height: 1rem;
    margin-bottom:.03rem;
    padding: .3rem 0 0 .4rem;
    border-bottom:1px solid  #F7F7F7;
}
.namme li:nth-of-type(2) {
    position: relative;
}
.namme li input {
    margin-left: .2rem;
    height: 100%;
    outline: none;
    border: none;
    font-size: .3rem;
    border-bottom:1px solid  #F7F7F7;

}
.btnn {
    width: 70%;
    height: 1.1rem;
    border-radius:10px;
    text-align: center;
    line-height: 1.1rem;
    background:linear-gradient(90deg,rgba(255,113,98,1) 0%,rgba(255,59,82,1) 100%);
    border-radius:44px;
    margin: .7rem auto;
    font-size: .35rem;
    color: #F7F7F7;
}
</style>
