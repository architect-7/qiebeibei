<template>
  <div id="orderTrack">
    <confirm
      v-model="showquxiao"
      :close-on-confirm="false"
      @on-confirm="confirm1">
      <p style="text-align:center;">您确定要取消该订单？</p>
    </confirm>
    <div class="order-status">
      <div v-if="obj.order_status=='0'"><img src="../../../assets/img/my/sdfk.png" align="absmiddle" alt=""> 待付款</div>
      <div v-if="obj.order_status=='1'"><img src="../../../assets/img/my/ddsjfh.png" align="absmiddle" alt=""> 等待卖家发货</div>
      <div v-if="obj.order_status=='2'"><img src="../../../assets/img/my/daifh.png" align="absmiddle" alt=""> 等待买家收货</div>
      <div v-if="obj.order_status=='3'"><img src="../../../assets/img/my/daipj.png" align="absmiddle" alt=""> 待评价</div>
      <div v-if="obj.order_status=='-3'"><img src="../../../assets/img/my/tksqz.png" align="absmiddle" alt=""> 退款申请中</div>
      <div v-if="obj.order_status=='-4'"><img src="../../../assets/img/my/tkywc.png" align="absmiddle" alt=""> 退款已完成</div>
    </div>
    <div class="content_top" >
      <div class="addressinfo">
        <i class="iconfont icon-locationfill"></i>
        <div class="div2">
          <div class="div1">
              <div class="left_name">{{obj.username}}</div>
              <div class="right_phone">{{obj.mobile}}</div>
          </div>
          <span>{{obj.province+obj.city+obj.district+obj.address}}</span>
        </div>
      </div>
      <i class="iconfont icon-right1 left"></i>
    </div>
    <div class="border">
      <img src="../../../assets/img/my/border.png" alt="">
    </div>
    <div class="buy-return" v-if="obj.order_status=='-3'">
      <div>退款时间 <span> {{obj.updated_at}}</span></div>
      <div>退款原因 <span> {{obj.return_content}}</span></div>
      <div>退款凭证 <img align="top" :src="obj.return_img" alt=""></div>
    </div>
    <div class="ordercont" >
      <div class="ordercont2">
        <div class="orderimg"><img :src="obj.goods_img" alt=""></div>
        <div class="orderinfo">
          <p>{{obj.goods_name}}</p>
          <p><span>￥{{obj.goods_price}}</span><span>x{{obj.goods_num}}</span></p>
        </div>
      </div>
      <div class="ordercont1">
        <div>运费</div>
        <div>￥{{obj.postage}}</div>
      </div>
      <div class="ordercont3">
        <div>商品总额</div>
        <div>￥{{obj.pay_price}}</div>
      </div>
      <div class="ordercont3 order-3">
        <div></div>
        <div>应付金额：<span class="s-1">￥{{obj.pay_price}}</span></div>
      </div>
    </div>
    <div class="order-box"></div>
    <div class="order-msg">
      <div class="order-t">订单信息</div>
      <div class="order-desc">
        <div>配送方式：<span>{{obj.expressName}}</span></div>
        <div>订单编号：<span>{{obj.order_id}}</span></div>
        <div style="display:flex;justify-content:space-between;align-items:center" v-if="obj.expressNo" ><span>快递单号：{{obj.expressNo}}</span> <span class="look-express" @click="jumpTo({name:'mallLogistics',query:{orderSn:obj.order_id}})">查看物流</span></div>
        <div>下单时间：<span>{{obj.created_at}}</span></div>
        <div>支付方式：<span>{{obj.pay_type=='credit'?'余额':''}}</span></div>
        <div>支付时间：<span>{{obj.pay_time}}</span></div>
      </div>
    </div>
    <div class="order-tab"  v-if="obj.order_status!=='3'">
      <div class="tab-right" >
        <div  v-if="obj.order_status!=='-3'"> </div>
        <div v-if="obj.order_status=='-3'" @click="linkService()"> <img src="../../../assets/img/my/dianhua.png" alt=""> 联系卖家</div>
        <div class="gray-bor" @click="handShow()"  v-if="obj.order_status=='0'">取消订单</div>
        <div class="gray-bor" @click="applyRefund()"  v-if="obj.order_status=='1'">申请退款</div>
        <div class="red-bor" @click="jumpTo({name:'mallUnderConfirm',query:{ model:obj.model,goodsId:obj.goods_id,changeValue:1}})" v-if="obj.order_status=='0'">去支付</div>
        <div class="gray-bor" @click="jumpTo({name:'orderProce',query:{orderId:check.orderSn}})"  v-if="obj.post_status=='1'">查看物流</div>
        <div class="red-bor" @click="confirmGood()"  v-if="obj.order_status=='2'">确认收货</div>
        <div class="red-bor" @click="goEvaluate()"  v-if="obj.order_status=='3'">去评价</div>
        <div class="gray-bor" @click="cancelReturn()"  v-if="obj.order_status=='-3'">撤销申请</div>
        <div class="gray-bor" @click="delOrder()"  v-if="obj.order_status=='-4'">删除订单</div>
      </div>
    </div>
  </div>
</template>

<script>
import * as apiHttp from '../../../api/index'
import {Confirm} from 'vux'
import {formatDate} from '../../../plugin/data'
export default {
  name: 'orderTrack',
  components: {
    Confirm
  },
  data(){
    return{
      isSearching: false,
      type: 0,
      data:this.$route.query.data,
      check:'',
      shopInfo:'',
      showquxiao:false,
      order:'',
      obj:{}
    }
  },
  mounted(){
    this.getDtails()
  },
   filters: {
    formatDate (time) {
      time = time * 1000
      var date = new Date(time)
      return formatDate(date, 'yyyy-MM-dd hh:mm')
    }
  },
  methods:{
    getDtails () {
      apiHttp.goodsOrderDetaiil(this.$route.query.order,'buy').then(response => {
        if (response.code === 0) {
          this.isShow = true
          this.obj = response.data.list[0]
          this.check  = response.data.order
          this.shopInfo = response.data.shopInfo
        }
      })
    },
    handShow(){
      this.order = this.obj.order_id
      this.showquxiao = true
    },
    cancelReturn(){
      this.$vux.toast.text("暂未开放");
    },
    delOrder(){
      apiHttp.delbuyOrder(this.obj.order_id).then(response => {
          if(res.code==0){
            this.$vux.toast.text(res.msg)
          }else{
            this.$vux.toast.text(res.msg)
          }
       })
    },
    applyRefund(){
      this.$router.push({name:'mallApplyRefund',query:{id:this.obj.order_id}})
    },
    goEvaluate(){
      this.$router.push({name:'mallUnderShopDetail',query:{id:this.obj.goods_id,type:this.obj.model}})
    },
    confirmGood(){
      apiHttp.secondConfirmGood(this.obj.order_id).then(response => {
          if(res.code==0){
            this.$vux.toast.text(res.msg)
          }else{
            this.$vux.toast.text(res.msg)
          }
       })
    },
    confirm1(){
      apiHttp.secondOrderRemove(this.order).then(response => {
        if (response.code === 0) {
          this.showquxiao = false
          this.$vux.toast.text(response.msg)
        } else {
          this.showquxiao = false
          this.$vux.toast.text('取消成功')
        }
      })
    },
    openExpress(){

    },
    linkService(){
        api.call({
          type: 'tel_prompt',
          number: this.obj.sell_phone
        })
    },
  }
}
</script>

<style lang="less" scoped>
#orderTrack{
  height: 100%;
  background: #f2f2f2;
  .order-status{
    width:100%;
    height:1.2rem;
    line-height: 1.2rem;
    background:url('../../../assets/img/gift/ddbj.png');
    div{
      font-size:0.35rem;
      font-weight:bold;
      color:white;
      padding-left:0.5rem;
      img{
        width:0.45rem;
        height:0.45rem;
      }
    }
  }
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
  .buy-return{
    width:100%;
    height:4rem;
    box-sizing: border-box;
    background:white;
    padding:0.3rem;
    font-size:0.3rem;
    font-weight:bold;
    color:#333333;
    div{
      margin-bottom:0.15rem;
    }
    span{
      color:#666666;
      font-weight:100;
    }
    img{
      width:1.6rem;
      height:2rem;
      border-radius:0.1rem;
    }
  }
  .ordercont{
    margin-top: 0.22rem;
    padding: 0 0.2rem;
    background:rgba(255,255,255,1);
    .ordercont1-1{
      height: 0.94rem;
      border-bottom: 0.01rem solid rgba(238,238,238,1);
      display: flex;
      align-items: center;
      justify-content: space-between;
      .service{
        border-radius:20px;
        border:1px solid #999999;
        padding-left:0.2rem;
        padding-right:0.2rem;
      }
      img{
        width:0.3rem;
        height:0.3rem;
      }
    }
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
          height:0.24rem;
          line-height: 0.24rem;
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
      border-bottom:1px solid #EDEDED;
      .s-1{
        color:#FF4D3A;
      }
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
      background:linear-gradient(90deg,rgba(249,87,77,1),rgba(238,26,35,1));
      font-size:0.32rem;
      color:rgba(255,255,255,1);
    }
  }
  .order-box{
    width:100;
    height:0.15rem;
    background:#F2F2F2;
  }
  .order-msg{
    width:100%;
    height:4.8rem;
    box-sizing: border-box;
    padding:0.3rem;
    background:white;
    border-bottom:8px solid #EDEDED;
    font-size:0.3rem;
    margin-bottom:1.2rem;
    .order-t{
      border-bottom:1px solid #EDEDED;
      padding-bottom:0.2rem;
      color:#333333;
    }
    .order-desc{
      height:0.6rem;
      line-height: 0.6rem;
      color:#666666;
      span{
        color:#666666
      }
      .look-express{
        display:inline-block;
        width:1.5rem;
        height:0.5rem;
        border-radius:0.1rem;
        text-align: center;
        line-height: 0.5rem;
        background:#01C2C7;
        color:white;
      }
    }
  }
  .order-vip{
      width:100%;
      box-sizing: border-box;
      padding:0.3rem;
      img{
        width:7rem;
      }
    }
  .order-tab{
    width:100%;
    height:1.2rem;
    background:white;
    box-sizing: border-box;
    padding-top:0.2rem;
    position: fixed;
    left:0;
    bottom:0;
    .tab-right{
      display:flex;
      width:100%;
      height:1rem;
      text-align: right;
      justify-content: space-between;
      align-items: center;
      box-sizing: border-box;
      padding:0.2rem;
      div{
        width:1.8rem;
        height:0.6rem;
        line-height: 0.6rem;
        text-align: center;
        border-radius:5px;
        
        img{
          width:0.35rem;
          height:0.35rem;
        }
      }
      .gray-bor{
        border:1px solid #999999;
      }
      .red-bor{
        background:#01C2C7;
        color:white;
      }
    }
  }
}
</style>
