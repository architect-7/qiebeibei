<template>
  <div class="giftDetail">
    <div class="gift-swiper">
      <swiper :options="swiperOption" ref="smallImages">
        <swiper-slide v-if="details.thumbs.length>0" class="goods_swiper" v-for="(item, index) in details.thumbs" :key="index">
          <img :src="item"  alt />
        </swiper-slide>
        <swiper-slide v-else  class="goods_swiper"  >
          <img :src="details.thumb" alt />
        </swiper-slide>
      </swiper>
      <img @click="goBack()" class="back-img" src="../../../assets/img/gift/back.png" alt />
    </div>
    <div class="gift-msg">
      <div class="row-msg">
        <div class="msg-t">{{details.goodsName}}</div>
        <div class="red-text">
            <div class="p-price">一口价：￥<span>{{details.spe_price}}</span></div>
            <div class="p-postage">邮费：￥{{details.freeShipping=='2'?details.postage:'0.00'}}</div>
            <div class="p-tag"><span v-for="(it,ind) in details.goodsTag" :key="ind">{{it}} </span> </div>
        </div>
      </div>
    </div>
    <div class="sell-shop">
      <div class="un-sell-1"><span></span> 卖家有话说</div>
      <div class="un-sell-2">
        <div v-html="details.info"></div>
      </div>
    </div>
    <div class="buy-sell-msg" v-if="msglist.length>0">
      <div class="un-sell-1"><span></span> 留言</div>
      <div class="un-sell-2" v-if="!isAll&&ind<1" v-for="(msg,ind) in msglist" :key="ind" @click="leaveMsg(msg.id)">
        <div class="leave-1">
           <div><img v-show="msg.avatar" :src="msg.avatar" alt=""><img v-show="!msg.avatar" src="../../../assets/img/logo.png" alt=""></div>
           <div>{{msg.name?msg.name:'暂无昵称'}}</div>
        </div>
        <div class="leave-2">{{msg.message}}</div>
        <div class="leave-3">
          <div style="color:#999999">{{msg.created_at}}</div>
          <div></div>
        </div>
        <div class="lea-answer" v-show="msg.list.length>0">
          <div v-for="(m,i) in msg.list" :key="i">
            <div class="answer-1">
              <div><img v-show="m.avatar" :src="m.avatar" alt=""><img v-show="!m.avatar" src="../../../assets/img/logo.png" alt=""></div>
              <div>{{m.name?m.name:'暂无昵称'}}</div>
            </div>
            <div class="answer-2">{{m.message}}</div>
            <div class="answer-3">
              <div style="color:#999999">{{m.created_at}}</div>
              <div></div>
            </div>
          </div>
        </div>
      </div>
      <div class="un-sell-2" v-if="isAll" v-for="(msg,ind) in msglist" :key="ind" @click="leaveMsg(msg.id)">
        <div class="leave-1">
          <div><img v-show="msg.avatar" :src="msg.avatar" alt=""><img v-show="!msg.avatar" src="../../../assets/img/logo.png" alt=""></div>
          <div>{{msg.name?msg.name:'暂无昵称'}}</div>
        </div>
        <div class="leave-2">{{msg.message}}</div>
        <div class="leave-3">
          <div style="color:#999999">{{msg.created_at}}</div>
          <div></div>
        </div>
        <div class="lea-answer" v-show="msg.list.length>0">
          <div v-for="(m,i) in msg.list" :key="i">
            <div class="answer-1">
              <div><img :src="m.avatar" alt=""></div>
              <div>{{m.name?m.name:'暂无昵称'}}</div>
            </div>
            <div class="answer-2">{{m.message}}</div>
            <div class="answer-3">
              <div style="color:#999999">{{m.created_at}}</div>
              <div></div>
            </div>
          </div>
        </div>
      </div>
      <div class="look-more" @click="lookMore()">{{more}} <img align="absmiddle" src="../../../assets/img/gift/bottom.png" alt=""></div>
    </div>
    <div class="gift-detail">
      <div>
        <img src="../../../assets/img/gift/giftdesc.png" alt />
      </div>
      <div class="desc" >{{details.goodsDesc}}</div>
    </div>
    <div class="gift-tab">
      <div class="small-text" @click="linkService()">
        <div>
          <img src="../../../assets/img/gift/lianxi.png" alt />
        </div>
        <div>联系卖家</div>
      </div>
      <div class="small-text" @click="leaveMsg()">
        <div>
          <img src="../../../assets/img/gift/scart.png" alt />
        </div>
        <div>留言</div>
      </div>
      <div class="white-text bynow" @click="send()">我想要</div>
    </div>
    <div class="leave-msg" v-if="mask" >
      <div class="l-box">
        <div>
          <input type="text" v-model="lmsg"> <span @click="sendMsg()">发送</span>
        </div>
      </div>
    </div>
    <div v-transfer-dom>
      <popup v-model="umask" position="bottom"  width="100%">
        <div class="detail_model">
            <div class="header">
              <div class="header_main">
                <div class="img_box">
                  <img :src="details.thumb" alt="">
                </div>
                <div class="right_main">
                  <div class="div1" >
                    <span>¥</span>{{details.spe_price}}
                  </div>
                 <div class="div2">邮费：{{details.freeShipping=='2'?details.postage:'0.00'}}</div>
                </div>
              </div>
            </div>
            <div class="num_main">
              <x-number title="购买数量" v-model="changeValue" width="0.5rem" :min="1" button-style="round" fillable ></x-number>
            </div>
            <div class="footer_buy">
              <div class="btn2"  @click="buyNow()">立即购买</div>
            </div>
           <div class="btn_close">
             <img src="../../../assets/mall/img/home/clone.png" alt="" @click="umask = false">
           </div>
        </div>
      </popup>
    </div>
  </div>
</template>

<script>
import { swiper, swiperSlide } from "vue-awesome-swiper";
import {Popup, TransferDom, XNumber, Popover} from 'vux'
export default {
  name: "underDetail",
  data() {
    return {
      id: this.$route.query.id,
      details: "",
      shopInfo: "",
      address: ' ',
      goodsNum:1,
      changeValue:1,
      lmsg:'',
      msglist:[],
      mid:'',
      isAll:false,
      mask:false,
      umask:false,
      more:'查看更多',
      type:this.$route.query.type,
      swiperOption: {
        pagination: {el: '.swiper-pagination'},
        paginationClickable: true,
        loop: false,
        autoplay: {
          delay: 1500,
          stopOnLastSlide: false,
          disableOnInteraction: false,
        },
        on: {
          transitionEnd: () => {
            if (this.$refs.smallImages.swiper) {
              this.index = this.$refs.smallImages.swiper.realIndex
            }
          },
          sliderMove: function (event) {
            window.canRightSlipBack = false
          },
          touchEnd: function (event) {
            setTimeout(() => {
              window.canRightSlipBack = true
            }, 600)
          }
        }
      },
      /*swiperOption: {
        grabCursor: false,
        loop: false,
        slidesPerView: 1,
       autoplay: {
          delay: 3000,
          disableOnInteraction: false
        },
        pagination: {
          el: ".swiper-pagination"
        },
        on: {}
      }*/
    };
  },
  components: {
    swiper,
    swiperSlide,
    Popup, 
    TransferDom,
    XNumber, 
    Popover
  },
  mounted() {
    this.getGiftDetail();
    this.getmsgList();
  },
  methods: {
    getGiftDetail() {
      this.$http.post("/mall/second-goods/goods-detail", {
         goodsId: this.id,
         model:this.type
        }).then(res => {
          this.details = res.data.list[0];
      });
    },
    getmsgList(){
      this.$http.post("/mall/second-goods/message-list", {
         model:this.type,goodsId: this.id 
        }).then(res => {
        this.msglist = res.data.list;
      });
    },
    lookMore(){
      if(this.isAll){
        this.$vux.toast.text("暂无更多数据");
        return;
      }
      this.isAll = true;
      this.more = '加载完毕'
    },
    send(){
      this.umask = true
    },
    buyNow(id) {
       this.$router.push({
            name: "mallUnderConfirm",
            query: {
                model:this.type,
                goodsId:this.id,
                changeValue:this.changeValue
            }
        });
    },
    linkService() {
      api.call({
        type: "tel_prompt",
        number: this.details.sell_phone
      });
    },
    leaveMsg(id){
      this.mask = true;
      this.mid = id
    },
    sendMsg(){
      if(!this.lmsg){
        this.$vux.toast.text("请填写留言内容");
        this.mask = false
        return;
      }
      this.$http.post("/mall/second-goods/stay-message", { 
          goodsId:this.id,
          stayId: this.mid,
          message:this.lmsg
        }).then(res => {
          if(res.code==0){
            this.mask = false
            this.lmsg = ''
            this.$vux.toast.text(res.msg)
            this.getmsgList()
          }else{
            this.$vux.toast.text(res.msg)
          }
      });
    },
    addCart() {
      this.$http
        .post("/shop/cart/add", { goodsId: this.id, goodsNum: 1, type: "add" })
        .then(res => {
          if (res.code == 0) {
            this.$vux.toast.text(res.msg);
          } else {
            this.$vux.toast.text(res.msg);
          }
        });
    }
  }
};
</script>

<style lang="less" scoped>
.giftDetail {
  width: 100%;
  box-sizing: border-box;
  font-size: 0.3rem;
  background: #ffffff;
  .leave-msg{
    width:100vw;
    height:1.5rem;
    background:rgba(0,0,0,0.1);
    position:fixed;
    bottom:0;
    left:0;
    z-index:9999;
    box-sizing: border-box;
    border-top:rgb(245,245,245);
    .l-box{
      width:100%;
      height:1.2rem;
      padding-top:0.5rem;
      background:white;
      text-align: center;
      input{
        width:5rem;
        height:0.5rem;
        border:1px solid rgb(245,245,245);
        border-radius:0.1rem;
        outline:none;
      }
      span{
        display:inline-block;
        width:1.5rem;
        height:0.5rem;
        line-height: 0.5rem;
        text-align: center;
        background:#01C2C7;
        color:white;
        border-radius:0.1rem;
      }
    }
  }
  .gift-swiper {
    img {
      width: 100%;
      height: 7rem;
    }
    .back-img {
      display: inline-block;
      width: 0.58rem;
      height: 0.58rem;
      background-size: 0.61rem 0.61rem;
      position: absolute;
      left: 0.3rem;
      top: 0.5rem;
      z-index: 999;
      text-align: center;
    }
  }
  .gift-msg {
    width: 100%;
    box-sizing: border-box;
    padding: 0.3rem;
    padding-bottom: 0px;
    .row-msg {
      width: 100%;
      div {
        margin-bottom: 0.1rem;
      }
      .red-text {
        width:100%;
        display:flex;
        justify-content: space-between;
        align-items: center;
        font-size:0.25rem;
        .p-price{
            color:#FF1D1C;
            span{
                font-size:0.4rem;
                font-weight:bold
            }
        }
        .p-postage{
            color:#666666;
        }
        .p-tag{
            span{
                width:0.8rem;
                height:0.4rem;
                text-align: center;
                line-height: 0.4rem;
                border-radius:0.1rem;
                background:#FFE9E9;
                color:#FF1D1C;
            }
        }
      }
      .msg-t {
        font-size: 0.35rem;
        font-weight: bold;
      }
      .msg-flex {
        display: flex;
        justify-content: space-between;
        align-items: center;
        div:first-child {
          color: #666666;
        }
        div:last-child {
          color: #999999;
        }
      }
    }
    .row-add {
      width: 100%;
      display: flex;
      justify-content: space-between;
      align-items: center;
      border-top: 1px solid #eeeeee;
      border-bottom: 1px solid #eeeeee;
      padding-top: 0.3rem;
      padding-bottom: 0.3rem;
      div:first-child {
        width:1rem;
        color: #666666;
      }
      div:last-child {
        flex:1;
        text-align: right;
        color: #999999;
      }
    }
    .row-post {
      width: 100%;
      padding-top: 0.3rem;
      padding-bottom: 0.3rem;
      span:first-child {
        color: #666666;
      }
      span:last-child {
        color: #666666;
      }
    }
  }
  .gift-box {
    width: 100%;
    height: 0.15rem;
    background: #f7f7f7;
  }
  .sell-shop{
    width:100%;
    height:3.5rem;
    background:white;
    border-top:8px solid #F2F2F2;
    .un-sell-1{
      width:100%;
      padding:0.3rem;
      color:#333333;
      font-weight:bold;
      box-sizing: border-box;
      border-bottom:1px solid #EEEEEE;
      span{
        display:inline-block;
        width:3px;
        height:0.3rem;
        border-radius:0.1rem;
        background:#01C2C7;
      }
    }
    .un-sell-2{
      width:100%;
      div{
        width:7rem;
        border-radius:0.1rem;
        background:#F8F8F8;
        box-sizing: border-box;
        color:#333333;
        padding:0.2rem;
        margin:0.3rem auto;
      }
      .leave-1{
        div{
          display:inline-block;
          font-weight:bold;
          img{
            width:0.6rem;
            height:0.6rem;
            border-radius:50%;
          }
        }
      }
    }
  }
  .buy-sell-msg{
    width:100%;
    background:white;
    border-top:8px solid #F2F2F2;
    .look-more{
      width:100%;
      height:0.8rem;
      line-height: 0.8rem;
      text-align: center;
      img{
        width:0.35rem;
        height:0.25rem;
      }
    }
    .un-sell-1{
      width:100%;
      padding:0.3rem;
      color:#333333;
      font-weight:bold;
      box-sizing: border-box;
      border-bottom:1px solid #EEEEEE;
      span{
        display:inline-block;
        width:3px;
        height:0.3rem;
        border-radius:0.1rem;
        background:#01C2C7;
      }
    }
    .un-sell-2{
      width:100%;
      padding:0.3rem;
      box-sizing: border-box;
      div{
        width:7rem;
        border-radius:0.1rem;
        box-sizing: border-box;
        color:#333333;
      }
      .leave-1{
        display:flex;
        color:#333333;
        font-weight:bold;
        height:0.8rem;
        line-height: 0.8rem;
        div:first-child{
          width:0.8rem;
          height:0.8rem;
          margin-right:0.3rem;
        }
        div{
          img{
            width:0.8rem;
            height:0.7rem;
            border-radius:50%;
          }
        }
      }
      .leave-2{
        padding-left:1rem;
        color:#666666;
        margin-bottom:0.1rem;
      }
      .leave-3{
        display:flex;
        justify-content: space-between;
        align-items: center;
        padding-left:1rem;
        color:#999999;
        font-size:0.25rem;
      }
      .lea-answer{
        width:7rem;
        box-sizing: border-box;
        padding:0.3rem;
        background:#F8F8F8;
        border-radius:0.1rem;
        margin:0.15rem auto;
        .answer-1{
          padding-left:0.5rem;
          display:flex;
          height:0.6rem;
          line-height: 0.6rem;
          font-weight:bold;
          div:first-child{
            width:1rem;
            margin-right:0.2rem;
          }
          img{
            width:0.6rem;
            height:0.6rem;
            border-radius:50%;
          }
        }
        .answer-2{
          padding-left:1.5rem;
        }
        .answer-3{
          padding-left:1.5rem;
          display:flex;
          justify-content: space-between;
          align-items: center
        }
      }
    }
  }
  .gift-detail {
    padding: 0.3rem;
    box-sizing: border-box;
    background: #ffffff;
    margin-bottom: 1.5rem;
    .desc {
      width: 100%;
      /deep/ img {
        max-width: 100%;
      }
    }
    img {
      width: 100%;
    }
  }
  .gift-tab {
    width: 100%;
    height: 1rem;
    box-sizing: border-box;
    background: white;
    position: fixed;
    left: 0px;
    bottom: 0px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    div {
      text-align: center;
    }
    .small-text {
      font-size: 0.2rem;
      color: #666666;
      width: 1.5rem;
    }
    .white-text {
      color: white;
    }
    .cart {
      width: 2.3rem;
      height: 1rem;
      line-height: 1rem;
      background: linear-gradient(to right, #ffcb00, #ff9702);
    }
    .bynow {
      width: 4.6rem;
      height: 1rem;
      line-height: 1rem;
      background: linear-gradient(to right, #01C2C7, #01C2C7);
    }
    img {
      width: 0.35rem;
      height: 0.35rem;
    }
  }
}
</style>
