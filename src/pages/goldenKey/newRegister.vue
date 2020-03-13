<template>
    <div id="lionn">
        <x-header slot="header" :left-options="{showBack: false}">
        <a @click="goBack()" slot="left" class="backBox"><i class="iconfont icon-back"></i></a>
        新用户注册
        <p style="position: absolute;right: .3rem;top: 0;color: rgba(153,153,153,1);font-size:.28rem;" @click="jumpTo('/newMember')">下载链接</p>
    </x-header>
        <!-- 实名 -->
        <div class="namme">
          <ul>
            <li>
              <span>用户名</span>
              <input type="text" placeholder="请输入用户名" v-model="userName">
            </li>
            <li>
              <span>密码</span>
              <input type="text" placeholder="请输入密码" v-model="password">
            </li>
            <li>
              <span>支付密码</span>
              <input type="text" placeholder="请输入支付密码" v-model="payPassword">
            </li>
            <li>
              <span>推荐人</span>
              <input type="text" placeholder="请输入推荐码" v-model="inviteName">
            </li>
            <li>
              <span>真实姓名</span>
              <input type="text" placeholder="请输入真实姓名" v-model="trueName">
            </li>
            <li>
              <span>身份证</span>
              <input type="text" placeholder="请输入身份证号" v-model="idCard">
            </li>
            <li>
              <span>手机号</span>
              <div>{{phonenum}}</div>
              <!-- <input type="text" placeholder="请输入注册手机号" v-model="phonenum"> -->
            </li>
          </ul>
        </div>
        <div class="btnn" @click="brnnn">确认</div>
    </div>
</template>

<script>
import axios from 'axios'
import store from '../../store'
import {Group, Actionsheet, XAddress, ChinaAddressV4Data , XInput, XHeader, Cell, Confirm, XDialog, XProgress, TransferDomDirective as TransferDom} from 'vux'
export default {
  name: 'newRegister',
  components: {
    Group, XInput, XHeader, Actionsheet, Cell, Confirm, XDialog, XProgress , XAddress
  },
  data () {
    return {
    //   userName:"ww",
    //   password:"qweqwe",
    //   payPassword:"123456",
    //   inviteName:"test1212",
    //   trueName:"李",
    //   idCard:"410426199605116018",
      userName:"",
      password:"",
      payPassword:"",
      inviteName:"",
      trueName:"",
      idCard:"",
      phonenum: '', 
      order_id: this.$route.query.order_id,
    }
  },
  mounted:function() {
    this.getinfo()
  },
  methods: {
    getinfo (){
       this.$http.post('/amoy/user/user-info', {}).then(res => {
        if(res.code ==0 && res.data.mobile!=''){
          this.phonenum = res.data.mobile
        }
      })
    },
    brnnn() {  //身份信息验证接口
      if(this.userName==""||this.password=="" || this.phonenum==""||
      this.payPassword==""||this.inviteName=="" || this.trueName==""|| this.idCard==""){
        this.$vux.toast.text('所有都要填写不能为空');
        return
      }else if(!(/^[1-9]{1}[0-9]{14}$|^[1-9]{1}[0-9]{16}([0-9]|[xX])$/.test(this.idCard))){
        this.$vux.toast.text('身份证号有误，请重新输入')
        return
      }else if(!(/^1(3|4|5|6|7|8|9)\d{9}$/.test(this.phonenum))){
        this.$vux.toast.text('手机号码有误，请重新输入')
        return
      }else{
          // 设置默认请求头
            axios.defaults.headers = {
            'X-Requested-With': 'XMLHttpRequest',
            'Content-type': 'application/x-www-form-urlencoded'
            }
            axios.post('https://app.puhui88.cn/mobile/api/reg_user',{
                userName:this.userName,
                password:this.password,
                mobile:this.phonenum,
                payPassword:this.payPassword,
                inviteName:this.inviteName,
                trueName:this.trueName,
                idCard:this.idCard,
            }).then(res=>{
                if(res.retCode== '1'){
                    this.$vux.toast.text(res.retMsg)
                    // this.jumpTo('/keypayMent?order_id='+this.order_id)
                    setTimeout(()=>{this.jumpTo('/newMember')},500)
                } else {
                    this.$vux.toast.text('用户不存，验证码不通过！')
                }
            })

            
      }
    
    }
  }

}
</script>
<style  lang="less">
.weui-actionsheet_toggle{
    bottom: 30px !important;
    width: 90% !important;
    left: 5% !important;
    background: none !important;
    .weui-actionsheet__menu{
    border-radius: 20px;
    }
    .weui-actionsheet__action{
    border-radius: 20px;
    color:rgba(33,153,255,1);
    height: 1rem;
    }
    .vux-actionsheet-menu-default{
    color: #666;
    font-size: .28rem;
    height: .8rem;
    line-height: .8rem
    }
    .weui-actionsheet__cell{
    font-size: .3rem;
    line-height: .8rem
    }
}
</style>
<style scoped >
#lionn {
  width: 100%;
  height: 100%;
  background-color: #fff;
}
.namme {
    width: 100%;
    font-size: .33rem;
    box-sizing: border-box;
    margin-top: .88rem;
}
.namme li {
    list-style: none;
    height: .65rem;
    line-height: .65rem;
    margin-bottom:.03rem;
    padding: .3rem 0 0 .4rem;
    border-bottom:1px solid  #F7F7F7;
    display: flex;
    justify-content: space-between;
    align-items: center;
}
.namme li span{
  font-size: .3rem;
}
.namme li input,
.namme li div{
    width: 5rem;
    margin-right: .2rem;
    height: 100%;
    outline: none;
    border: none;
    font-size: .28rem;
    text-align: right;
}
.namme li .img_left{
    margin-right: .2rem;
    width: .16rem;
    height: .28rem;
}
.namme li .span_left{
    margin-right: .2rem;
    font-size: .3rem;
    font-weight:500;
    color:rgba(255,54,53,1); 
}
.btnn {
    width: 60%;
    height: .8rem;
    border-radius:10px;
    text-align: center;
    line-height: .8rem;
    background:linear-gradient(90deg,#3FE7E5 0%,#10A9B0 100%);
    border-radius:5px;
    margin: 1rem auto;
    font-size: .32rem;
    color: #F7F7F7;
}
</style>
