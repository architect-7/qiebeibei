<template>
  <div class="giftDetail">
      <div class="gift-swiper">
        <swiper :options="swiperOption" ref="smallImages">
          <swiper-slide class="goods_swiper" v-for="(item, index) in list.thumbs" :key="index">
            <img :src="item" alt="">
          </swiper-slide>
        </swiper>
        <div class="swiper-pagination" slot="pagination"></div>
      </div>
      <div class="gift-msg" >
          <div class="row-post">
              <p>{{list.goodsName}}</p>
          </div>
      </div>
      <div class="gift-box"></div>
      <div class="gift-msg" >
          <div class="row-post">
            <p style="color:#999999">
              <span style="color:#FF1D1C;margin-right:.2rem;font-weight:border;">￥{{list.spe_price}}</span>
              <del>￥{{list.marketprice}}</del>
            </p> 
          </div>
      </div>
      <div class="gift-box"></div>
      <div class="gro-right" ></div>
      <div class="gift-detail">
          <div><img src="../../assets/img/gift/giftdesc.png" alt=""></div>
          <div class="desc" v-html="list.info"></div>
      </div>
      <div class="gift-tab">
          <div class="white-text bynow" @click="gobug()">立即购买</div>
      </div>
  </div>
</template>

<script>
import * as utils from '../../utils'
import {swiper, swiperSlide} from 'vue-awesome-swiper'
import * as apiHttp from '../../api/index'
export default {
  name: 'goldenKeykeyDetail',
  data (){
      return {
          list:[],
          swiperOption: {
            grabCursor: true,
            loop: true,
            slidesPerView: 1,
            autoplay: {
            delay: 3000,
            disableOnInteraction: false
            },
            pagination: {
            el: ".swiper-pagination",
            type: 'fraction'
            },
            on: {
            
            }
        }
      }
  },
  components:{
      swiper,
      swiperSlide
  },
  mounted (){
     this.list = utils.storage.get('keykeyDetail')
  },
  methods:{
    gobug () {
      this.jumpTo('/keyfirmOrder')
    }
  }
}
</script>

<style lang="less" scoped>
.gift-swiper{
  width: 100%;
  height: 7.12rem;
  overflow: hidden;
  .swiper-pagination{
    position:relative;
    bottom:1rem;
    left: 100%;
    margin-left: -1.3rem;
    width:0.8rem;
    height:0.8rem;
    border-radius: 50%;
    background:rgba(255,255,255,.5);
    color:#000;
    font-size:0.28rem;
    text-align: center;
    line-height:0.8rem;
  }
}
.giftDetail{
    width:100%;
    box-sizing: border-box;
    font-size:0.3rem;
    position:relative;
    top:0;
    left:0;
    .all-list{
        width:7rem;
        height:1rem;
        /*line-height: 1rem;*/
        text-align: center;
        font-weight:bold;
        color:#333333;
        .all-close{
            position:absolute;
            right:1rem;
            top:2.2rem;
            width:0.5rem;
            height:0.5rem;
        }
        .all-flex{
            box-sizing: border-box;
            padding:0.2rem;
            width:6rem;
            height:1.2rem;
            display:flex;
            justify-content: space-between;
            align-items: center;
            border-bottom:1px solid #EEEEEE;
            .pin-now{
                width:1.2rem;
                height:0.5rem;
                border-radius:0.1rem;
                background:#FF1D1C;
                line-height: 0.5rem;
                color:white;
                text-align: center
            }
            img{
                width:0.7rem;
                height:0.7rem;
                border-radius:50%;
            }
        }
    }
    .group-msg{
        width:7rem;
        height:3.4rem;
        border-radius:0.2rem;
        position:absolute;
        left:0.25rem;
        top:6.2rem;
        z-index:999;
        .gro-1{
            width:100%;
            height:1rem;
            background:url('../../assets/img/group/tuangougj.png');
            background-size:100% 1rem;
            .g-left{
                color:white;
                display:inline-block;
                width:35%;
                text-align:center;
                div:first-child{
                    font-size:0.2rem;
                    padding-top:0.1rem;
                }
                div:last-child{
                    font-weight:bold;
                    font-size:0.35rem;
                }
            }
        }
        .gro-msg{
            width:100%;
            background:white;
            border-radius:0.2rem;
            box-sizing: border-box;
            padding:0.2rem;
            .gro-m-1{
                font-size:0.3rem;
                color:#333333;
            }
            .gro-m-2{
                width:100%;
                display:flex;
                justify-content: space-between;
                align-items: center;
                margin-top:0.2rem;
                margin-bottom:0.2rem;
                color:#616161;
                span{
                    color:#FF1D1C;
                }
                .icon{
                    width:1.6rem;
                    height:0.5rem;
                    border:1px solid #01C2C7;
                    border-radius:0.1rem;
                    span{
                        display:inline-block;
                        text-align: center;
                        line-height: 0.5rem;
                        color:#01C2C7;
                    }
                    .s-2{
                        width:1rem;
                    }
                    .s-1{
                        width:0.5rem;
                        height:0.5rem;
                        line-height: 0.5rem;
                        background:#01C2C7;
                        img{
                            width:0.4rem;
                            height:0.4rem;
                        }
                    }
                }
            }
            .gro-m-3{
                color:#A5A5A5;
                font-size:0.25rem;
            }
        }
    }
    .gift-swiper{
        img{
            width:100%;
            height:7rem;
        }
        .back-img{
            display: inline-block;
            width: 0.58rem;
            height: 0.58rem;
            background-size: 0.61rem 0.61rem;
            position: absolute;
            left: 0.3rem;
            top:0.3rem;
            z-index: 999;
            text-align: center;
        }
    }
    .gift-msg{
        width:100%;
        box-sizing: border-box;
        padding-bottom:0px;
        background:white;
        padding-left:0.3rem;
        .row-msg{
            width:100%;
            div{
                margin-bottom:0.1rem;
            }
            .red-text{
                color:#FF1D1C;
                font-weight:bold
            }
            .msg-t{
                font-size:0.35rem;
                font-weight:bold;
            }
            .msg-flex{
                display:flex;
                justify-content: space-between;
                align-items: center;
                div:first-child{
                    color:#666666;
                }
                div:last-child{
                    color:#999999
                }
            }
        }
        .row-add{
            width:100%;
            display:flex;
            justify-content: space-between;
            align-items: center;
            border-top:1px solid #EEEEEE;
            border-bottom:1px solid #EEEEEE;
            padding-top:0.3rem;
            padding-bottom:0.3rem;
            div:first-child{
                color:#666666;
            }
            div:last-child{
                color:#999999;
            }
        }
        .row-post{
            width:100%;
            padding-top:0.3rem;
            padding-bottom:0.3rem;
            p{
                font-size: .28rem;
                color: #2F2F2F;
                display: -webkit-box;
                -webkit-box-orient: vertical;
                -webkit-line-clamp: 2;
                overflow: hidden;
            }
        }
    }
    .gift-box{
        width:100%;
        height:0.15rem;
        background:#F7F7F7;
    }
    .look-share{
        width:100%;
        box-sizing: border-box;
        padding:0.3rem;
        display:flex;
        justify-content: space-between;
        align-items: center;
        background:white;
        div:first-child{
            color:#333333
        }
        div:last-child{
            color:#999999;
            font-size:0.2rem;
        }
    }
    .gift-detail{
        padding:0.3rem;
        box-sizing: border-box;
        background:#FFFFFF;
        margin-bottom:1rem;
        .desc{
            width:100%;
            /deep/ img{
              width: 100% !important;
              display: block;
            }
        }
        img{
            width:100%;
        }
    }
    .gift-tab{
        width:100%;
        height:1rem;
        box-sizing: border-box;
        background:white;
        position:fixed;
        left:0px;
        bottom:0px;
        display:flex;
        justify-content: space-between;
        align-items: center;
        div{
            text-align: center;
        }
        .white-text{
          color:white;
        }
        .bynow{
            width:100%;
            height:1rem;
            line-height: 1rem;
            background: linear-gradient(to right,#F9574D, #EE1A23);
        }
    }
}
#all-mask{
    width:100vw;
    height:100vh;
    background:rgba(0,0,0,0.5);
    position:fixed;
    top:0px;
    left:0px;
    z-index:1000;
    .all-list{
        width:6.2rem;
        height:7rem;
        border-radius:5px;
        /*position:absolute;
        top:1rem;
        left:0.5rem;*/
        margin-top:2rem;
        margin:2rem auto;
        background:white;
    }
}
@keyframes aa {
   /* from {
      transform: translateY(0)
    }
    to {
      transform: translateY(-6rem)
    }
    from {
      transform: translateY(0)
    }
    0% {             
        transform: translate(0px, 0px);            
    }            
    50% {                
        transform: translate(0px, 1rem);            
    }            
    100% {                
        transform: translate(0px, 0px);
    }*/
     0% {
            transform: translateY(0px);
            -webkit-transform: translateY(0px);
        }
        100% {
            transform: translateY(-200%);
            -webkit-transform: translateY(-200%);
        }
  }
  #lunbo {
    position: relative;
    top:75vw;
    width: 100%;
    margin-bottom:10px;
  }
  .mark {
    /* 会员才会隐去 */
    display: block;
    position: absolute;
    box-sizing: border-box;
    top: -5.4rem;
    left: 0;
    width: 100%;
    height: 4.3rem !important;
    background: white;
    text-align: center;
    color: black;
    overflow: hidden;
  }

  .mark .gro-item {
    font-weight: 500;
    color: black;
    height:1.5rem;
    width:100%;
    padding:0.2rem;
    box-sizing: border-box;
    font-size: .3rem;
    display:flex;
    justify-content: space-between;
    align-items: center;
    border-bottom:1px solid #EEEEEE;
    animation: aa 5s linear infinite;
  }
  .mark .gro-item img{
      width:0.65rem;
      height:0.65rem;
      border-radius:50%
  }
  .mark .gro-item .gro-right div{
      display:inline-block;
  }
  .mark .gro-item .gro-right p:last-child{
      color:#999999;
      font-size:0.25rem;
  }
   .crowdor{
      width:1.2rem;
      height:0.5rem;
      border-radius:0.1rem;
      line-height: 0.5rem;
      text-align: center;
      color:white;
      background:#FF1D1C; 
  }
</style>
