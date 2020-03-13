<template>
    <div id="lionn">
        <!-- 实名 -->
        <div class="namme">
          <ul>
            <li>
              <span>昵称</span>
              <input type="text" placeholder="请输入昵称" v-model="tname">
            </li>
            <li>
              <span>身份证</span>
              <input type="text" placeholder="请输入身份证号" v-model="idcard">
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
import * as utils from '../../utils'
import {Group, Actionsheet, XAddress, ChinaAddressV4Data , XInput, XHeader, Cell, Confirm, XDialog, XProgress, TransferDomDirective as TransferDom} from 'vux'
export default {
  name: 'oldMember',
  components: {
    Group, XInput, XHeader, Actionsheet, Cell, Confirm, XDialog, XProgress , XAddress
  },
  data () {
    return {
      tname:"",
      idcard:"",
      phonenum: '', 
      addid: this.$route.query.address_id,
      keylist: utils.storage.get('keykeyDetail')
    }
  },
  mounted:function() {
    this.getinfo()
    console.log(this.keylist)
    console.log(this.keylist.id,this.addid)
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
      if(this.tname==""||this.idcard=="" || this.phonenum==""){
        this.$vux.toast.text('所有都要填写不能为空');return
      }else if(!(/^[1-9]{1}[0-9]{14}$|^[1-9]{1}[0-9]{16}([0-9]|[xX])$/.test(this.idcard))){
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
            axios.post('https://app.puhui88.cn/mobile/api/get_user_if_exist',{
                userName:this.tname,
                idCard:this.idcard,
                mobile:this.phonenum
            }).then(res=>{
                if(res.retData.ifExist){
                    console.log(res.retData.ifExist == 'true')
                    this.establishOrder()
                } else {
                    this.$vux.toast.text('用户不存，验证码不通过！')
                }
            })

            
      }
    },
    establishOrder () {
      this.$http.post('/mall/gold/add-order',{
        id: this.keylist.id,
        address_id: this.addid,
        gold_user_name: this.tname,
        gold_card: this.idcard
        }, true, true).then((res) => {
          if (res.code==0) {
              console.log(res.data.order_id)
              // this.jumpTo('/oldMember?order_id='+res.data.order_id) 
              this.jumpTo('/keypayMent?order_id='+res.data.order_id)
          }
      })
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
.namme li div {
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
