<template>
  <div class="payMent">
    <div class="header">
      <x-header
        slot="header"
        :left-options="{showBack: false}"
      >
        <a @click="goBack()" slot="left" class="backBox"><i class="iconfont icon-back"></i></a>
        支付
      </x-header>
    </div>
    <div class="content">
      <div class="payMent_parse">
        <div class="left">
          订单金额
        </div>
        <div class="right">
          <span>¥</span>{{list.spe_price}}
        </div>
      </div>
      <div class="payMent_list">
        <div class="list_title">
          选择支付方式
        </div>
        <div class="list"  @click="handChange('integral')" >
          <div class="img_box">
            <img src="../../assets/mall/img/home/yue.png" alt="">
            <span>积分</span>
          </div>
          <div class="checkbox">
            <img :src="active == 'integral' ? './static/libs/img/active.png' : './static/libs/img/active_no.png'" alt="">
          </div>
        </div>
         <div class="list"  @click="handChange('alipay')" >
          <div class="img_box">
            <img src="../../assets/mall/img/home/apliy.png" alt="">
            <span>支付宝</span>
          </div>
          <div class="checkbox">
            <img :src="active == 'alipay' ? './static/libs/img/active.png' : './static/libs/img/active_no.png'" alt="">
          </div>
        </div>
         <div class="list"  @click="handChange('wechat')" >
          <div class="img_box">
            <img src="../../assets/mall/img/home/wheat.png" alt="">
            <span>微信</span>
          </div>
          <div class="checkbox">
            <img :src="active == 'wechat' ? './static/libs/img/active.png' : './static/libs/img/active_no.png'" alt="">
          </div>
        </div>
      </div>
      <div class="btns">
        <div class="btn" @click="handBuy">立即购买</div>
      </div>
    </div>
  </div>
</template>

<script>
import * as utils from '../../utils'
import * as apiHttp from '../../api/index'
import axios from 'axios'
export default {
  name: 'keypayMent',
  data () {
    return {
      active: 'integral',
      data: {},
      payType: {},
      oilCard:this.$route.query.oilCard,
      order_id:this.$route.query.order_id,
      list: utils.storage.get('keykeyDetail'),
      credit1: ''
    }
  },
  mounted: function () {
    // 删除
    this.getjifen()
  },
  methods: {
    getjifen () {
       this.$http.post('/amoy/user/user-info', {}, true).then(res => {
         if (res.code == 0){
           this.credit1 = res.data.credit1
         }
       })
    },
    handChange (key) {
      this.active = key
    },
    // 点击支付
    getWay (type) {
      this.$http.post('/mall/gold/gold-pay', {
        order_id: this.order_id,
        pay_type: type
      }, true).then(res => {
        this.dialog = false
        if (res.code === 0) {
          if (type === 'alipay') {
            this.plusReady(1, res.data.payOrder)
          } else {
            this.plusReady(0, res.data.payOrder)
          }
        }
      })
    },
    // 获取支付通道
    plusReady (index, data) {
      let that = this
      if (index === 1) {
        let aliPayPlus
        if (this.$aliPay === 1) {
          aliPayPlus = api.require('aliPayPlus')
        } else {
          aliPayPlus = api.require('aliPayPlus')
        }
        aliPayPlus.payOrder({
          orderInfo: data
        }, function (ret, err) {
         
          switch (ret.code) {
            case '9000':
              that.$vux.toast.text('支付成功')
              that.$router.push({
                name: 'PaySuccess'
              })
              break
            case '6001':
              that.$vux.toast.text('取消支付')
              break
            case '8000':
              that.$vux.toast.text('正在处理中')
              break
            default:
              that.$vux.toast.text('支付失败')
          }
        })
      }else if (index === 0) {
        
        let wxPay = api.require('wxPay')
        let info = {
          apiKey: data.appid,
          orderId: data.prepayid,
          mchId: data.partnerid,
          nonceStr: data.noncestr,
          timeStamp: data.timestamp,
          sign: data.sign
        }
        wxPay.payOrder(info, function (ret, err) {
          if (ret.status) {
            that.$vux.toast.text('支付成功')
            that.$router.push({
              name: 'PaySuccess'
            })
          } else {
           
            if (err.code === -2) {
              that.$vux.toast.text('取消支付')
            } else {
              that.$vux.toast.text('支付失败')
            }
          }
        })
      }
    },
    handBuy () {
      console.log(this.active)
      if (this.active === 'integral') {
        if (Number(this.list.spe_price)<=Number(this.credit1)) {
            this.$http.post('/mall/gold/gold-pay', {
            order_id: this.order_id,
            pay_type: this.active
          }, true).then(res => {
            if (res.code == 0) {
              this.$vux.toast.text('支付成功')
              this.$router.push({
                name: 'PaySuccess'
              })
            }
          })
        } else {
          this.$vux.toast.text('余额不足！')
        }
      }else{
        this.getWay(this.active)
      }

    }
  }
}
</script>

<style scoped>

</style>
