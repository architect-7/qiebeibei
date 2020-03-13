<template>
  <div id="confirmorders">
    <div v-show="check.address==''" class="content_top" @click="handAddress()">
        点击添加默认地址
    </div>
    <div v-show="check.address" class="content_top" @click="jumpTo('/mall/address')">
      <div class="addressinfo">
        <i class="iconfont icon-locationfill"></i>
        <div class="div2">
          <div class="div1">
              <div class="left_name">{{check.address ?check.address.username:''}}</div>
              <div class="right_phone">{{check.address ?check.address.mobile:''}}</div>
          </div>
          <span>{{check.address ?check.address.province+check.address.city+check.address.district+check.address.address:''}}</span>
        </div>
      </div>
      <i class="iconfont icon-right1 left"></i>
    </div>
    <div class="border">
      <img src="../../../assets/img/my/border.png" alt="">
    </div>
    <div v-if="check.list">
      <div class="ordercont" v-for="(item,index) in check.list" :key="index">
        <div class="ordercont2">
          <div class="orderimg"><img :src="item.thumb" alt=""></div>
          <div class="orderinfo">
            <p>{{item.goodsName}}</p>
            <p><span>￥{{item.spe_price}}</span><span>x{{changeValue}}</span></p>
          </div>
        </div>
        <div class="ordercont1" v-if="item.freeShipping=='2'">
          <div>运费</div>
          <div>￥{{item.postage}}</div>
        </div>
        <div class="ordercont3">
          <div>商品总计</div>
          <div>￥{{check.totalMoney}}</div>
        </div>
      </div>
    </div>
    <div class="pay-type"><span></span> 支付方式</div>
    <div class="confirm-pay">
        <div><img src="../../../assets/img/my/yue.png" align="absmiddle" alt=""> 余额支付</div>
        <div><img src="../../../assets/img/my/conxz.png" align="absmiddle" alt=""></div>
    </div>
    <div class="submitorder">
      <div>
        <span>合计：</span><span>￥{{check.totalMoney}}</span>
      </div>
      <div @click="submitOrder()">提交订单</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'underConfirm',
  data(){
    return{
      isSearching: false,
      type: 0,
      data:this.$route.query.data,
      goodsId:this.$route.query.goodsId,
      goodsNum:1,
      changeValue:this.$route.query.changeValue,
      model:this.$route.query.model,
      check:{}
    }
  },
  mounted(){
    this.checkout()
  },
  methods:{
    checkout(){
      this.$http.post('/mall/second-goods/goods-detail',{
        model: this.model,
        goodsId: this.goodsId,
        checkOut :'checkOut',
        goodsNum:this.changeValue
      }).then(res => {
        this.check = res.data          
      })
    },
    handAddress(){
      this.$router.push({
        name:'mallAddAddress'
      })
    },
    submitOrder(){
      if(!this.check.address){
        this.$vux.toast.text("请选择地址");
        return;
      }
    this.$http.post("/mall/second-goods/create", { 
        goodsId: this.goodsId ,
        goodsNum:this.changeValue
     }).then(res => {
        if (res.code == 0) {
            this.$http.post("/mall/second-goods/pay-order", { 
                orderId: res.data ,
                payType: 'credit'
            }).then(res => {
                if (res.code == 0) {
                    this.$vux.toast.text(res.msg)
                    this.$router.replace({
                        name:'mallMy'
                    })
                }else{
                    this.$vux.toast.text(res.msg)
                }
            })
        }else{
            this.$vux.toast.text(res.msg)
        }
     })
    }
  }
}
</script>

<style lang="less" scoped>
#confirmorders{
  height: 100%;
  background: #f2f2f2;
  .content_top{
    padding:0.35rem 0.3rem;
    background:#fff;
    font-size: 0.26rem;
    display: flex;
    align-items: center;
    justify-content: space-between;
    .addressinfo{
      display: flex;
      align-items: center;
      i{
        margin-right: 0.26rem;
      }
      .div1{
        display: flex;
        align-items: center;
        height:0.28rem;
        line-height: 0.28rem;
        margin-bottom: 0.28rem;
        font-size:0.30rem;
        color:rgba(26,26,26,1);
        .right_phone{
          margin-left: 0.36rem;
        }
      }
      .div2{
        display: flex;
        flex-direction: column;
        &>span{
          height:0.23rem;
          line-height: 0.23rem;
          font-size:0.24rem;
          color:rgba(102,102,102,1);
        }
      }
    }
  }
  .border{
    width:100%;
    height:0.06rem;
    img{
      width:100%;
      height:0.06rem;
      position:relative;
      top:-0.3rem;
    }
  }
  .pay-type{
      width:100%;
      height:1rem;
      font-size:0.31rem;
      color:#333333;
      font-weight:bold;
      padding:0.3rem;
      box-sizing: border-box;
      span{
          display:inline-block;
          height:0.3rem;
          width:3px;
          border-radius:3px;
          background:#01C2C7;
      }
  }
  .confirm-pay{
      width:100%;
      padding:0.3rem;
      box-sizing: border-box;
      height:1rem;
      background:white;
      display:flex;
      justify-content: space-between;
      align-items: center;
      color:#333333;
      div:first-child{
          img{
              width:0.6rem;
              height:0.6rem;
          }
      }
      div:last-child{
          img{
              width:0.35rem;
              height:0.35rem;
          }
      }
  }
  .ordercont{
    margin-top: 0.22rem;
    padding: 0 0.2rem;
    background:rgba(255,255,255,1);
    .ordercont1{
      height: 0.94rem;
      border-bottom: 0.01rem solid rgba(238,238,238,1);
      display: flex;
      align-items: center;
      justify-content: space-between;
      div{
        height:0.28rem;
        line-height: 0.28rem;
        font-size:0.3rem;
        color:#333;
      }
      &>div:last-child{
        height:0.28rem;
        line-height: 0.28rem;
        font-size:0.30rem;
        color:#666;
      }
    }
    .ordercont2{
      padding: .4rem 0 .15rem;
      display: flex;
      .orderimg{
        width:1.6rem;
        height:1.6rem;
        margin-right: 0.18rem;
        border-radius:0.1rem;
        &>img{
          width: 100%;
          height: 100%;
        }
      }
      .orderinfo{
        flex: 1;
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        &>p:nth-child(1){
          line-height: 0.3rem;
          margin: 0.1rem 0;
          font-size:0.26rem;
          color:rgba(51,51,51,1);
        }
        &>p:nth-child(2){
          height:0.23rem;
          line-height: 0.23rem;
          margin: 0.1rem 0;
          font-size:0.3rem;
          color:rgba(255,88,71,1);
          display: flex;
          justify-content: space-between;
          span:last-child{
            height:0.2rem;
            font-size:0.26rem;
            color:rgba(153,153,153,1);
          }
        }
      }
    }
    .ordercont3{
      height: 1.05rem;
      display: flex;
      justify-content: space-between;
      align-items: center;
      div{
        height:0.28rem;
        line-height: 0.28rem;
        font-size:0.3rem;
        color:#333;
      }
      &>div:last-child{
        height:0.28rem;
        line-height: 0.28rem;
        font-size:0.30rem;
        color:#666;
      }
    }
  }
  .submitorder{
    background: white;
    display: flex;
    align-items: center;
    justify-content: space-between;
    position: fixed;
    bottom: 0;
    left: 0;
    width: 100%;
    &>div:first-child{
      margin-left: 0.26rem;
      height:0.29rem;
      font-size:0.3rem;
      color:rgba(0,0,0,1);
      &>span:last-child{
        color: #FF4D3A;
      }
    }
    &>div:last-child{
      width:2.70rem;
      height:0.98rem;
      line-height: 0.98rem;
      text-align: center;
      background:linear-gradient(90deg,#01C2C7,#01C2C7);
      font-size:0.32rem;
      color:rgba(255,255,255,1);
    }
  }
}
</style>
