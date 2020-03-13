<template>
  <div class="giftDetail">
      <div id="all-mask" v-if="mask">
        <div class="all-list">
            <div class="all-tit" style="line-height:1rem;background:#F7F7F7;">加入拼单<img @click="mask=false" class="all-close" src="../../assets/img/gift/guanbi.png" alt=""></div>
            <div style="overflow-y: scroll;height:6rem">
                <div class="all-flex" v-for="(item,index) in groupList" :key="index">
                    <div><img align="absmiddle" :src="item.avatar?item.avatar:'./static/img/auth/logo.png'" alt=""></div>
                    <div>
                        <div>{{item.nickName}} &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="font-size:0.2rem;color:#666666">还差{{parseInt(item.should_team_num)-parseInt(item.real_team_num)}}人拼成</span> </div>
                        <div style="color:#999999">{{item.created_at}}</div>
                    </div>
                    <div class="pin-now" @click="addOrder(item)">去拼单</div>
                </div>
            </div>
        </div>
        <div class="all-join"></div>
      </div>
      <div class="gift-swiper">
        <swiper :options="swiperOption" ref="smallImages">
          <swiper-slide class="goods_swiper" v-for="(item, index) in details.thumbs" :key="index">
            <img :src="item" alt="">
          </swiper-slide>
        </swiper>
      </div>
      
      <div class="group-msg">
          <div class="gro-1">
              <div class="g-left">
                  <div>拼团价</div>
                  <div>￥{{details.group_buy_price}}</div>
              </div>
              <div></div>
          </div>
          <div class="gro-msg">
              <div class="gro-m-1">{{details.goodsName}}</div>
              <div class="gro-m-2">
                  <div class="icon"><span class="s-1"><img align="absmiddle" src="../../assets/img/group/2rentuan.png" alt=""></span><span class="s-2">2人团</span></div>
                  <div>仅剩 <span>{{details.total}}</span> 件</div>
              </div>
              <div class="gro-m-3"> <del>单买：￥{{details.spe_price}}</del> </div>
           </div>
      </div>
      <div style="height:2.5rem;"></div>
      <div class="gift-msg" >
          <div class="row-post">
              <span>运费</span> <span>包邮</span>
          </div>
      </div>
      <div class="gift-box"></div>
       <div class="look-share" >
           <div>{{groupList.length}}正在拼团,可直接参与</div>
           <div @click="lookAll()">查看全部 ></div>
       </div>
       <div id="lunbo" v-if="groupList.length>0">
        <div class="mark"  :style="{height:itemh*groupList.length+'rem !important'}">
          <div class="gro-item" v-for="(item,index) in groupList" :key="index">
            <div >
                <img align="absmiddle" :src="item.avatar?item.avatar:'./static/img/auth/logo.png'" alt=""> <span>&nbsp; {{item.nickName?item.nickName:'暂无昵称'}}</span>
            </div>
            <div class="gro-right" >
                <p>还差{{parseInt(item.should_team_num)-parseInt(item.real_team_num)}}人拼成</p>
                <p >{{item.created_at}}</p>
            </div>
            <div class="crowdor" @click="addOrder(item)">去拼单</div>
          </div>
        </div>
       </div>
      <div :style="{height:itemh*groupList.length+'rem !important'}" v-if="groupList.length>0"></div>
      <div class="gift-detail">
          <div><img src="../../assets/img/gift/giftdesc.png" alt=""></div>
          <div class="desc" v-html="details.info"></div>
      </div>
      <div class="gift-tab">
          <div class="small-text" @click="handShou(details.shopInfo.mobile)">
              <div><img src="../../assets/img/gift/lianxi.png" alt=""></div>
              <div>联系客服</div>
          </div>
          <div class="white-text cart" @click="createOrder()">单独购买</div>
          <div class="white-text bynow" @click="createOrder(1)">我要拼团</div>
      </div>
  </div>
</template>

<script>
import {swiper, swiperSlide} from 'vue-awesome-swiper'
import * as apiHttp from '../../api/index'
export default {
  name: 'gDetail',
  data (){
      return {
          id:this.$route.query.id,
          details:'',
          shopInfo:'',
          itemh:1.2,
          groupList:[],
          mask:false,
          swiperOption: {
            grabCursor: true,
            loop: true,
            slidesPerView: 1,
            autoplay: {
            delay: 3000,
            disableOnInteraction: false
            },
            pagination: {
            el: ".swiper-pagination"
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
      this.getGiftDetail();
      this.getGroupDetail();
  },
  methods:{
      handShou (mobile) {
        api.call({
          type: 'tel_prompt',
          number: mobile
        })
      },
      addOrder(item){
          apiHttp.cardInterim(this.id, 1, 0).then(response => {
          if (response.code === 0) {
            this.$vux.toast.text('参团成功')
            this.$router.push({
              name: 'mallFirmOrder',
              query: {
                data: response.data,
                groupType:1,
                GroupId : item.id,
                addGroup:1
              }
            })
          }
        })
      },
      getGiftDetail(){
        this.$http.post('/mall/index/goods-detail',{goodsId:this.id}).then(res => {
          this.details = res.data.detail
          this.shopInfo = res.data.detail.shopInfo
        })
      },
      lookAll(){
          if(this.groupList.length>0){
              this.mask = true
          }else{
              this.$vux.toast.text("暂无拼团信息");
          }
      },
      createOrder(type){
         apiHttp.cardInterim(this.id, 1, 0).then(response => {
          if (response.code === 0) {
            this.$router.push({
              name: 'mallFirmOrder',
              query: {
                data: response.data,
                groupType:type
              }
            })
          }
        })
      },
      getGroupDetail(){
        this.$http.post('/mall/group/group-list',{goodsId:this.id}).then(res => {
            this.groupList = res.data.list;
        })
      },
      buyNow(id){
          this.$http.post('/shop/cart/interim',{goodsId:this.id}).then(res => {
            if(res.code==0){
                this.$router.push({
                    name: 'confirmorder',
                    query:{
                        data:res.data
                    }
                })
            }else{
                this.$vux.toast.text(res.msg);
            }
        })
      },
      linkService(){
          api.call({
          type: 'tel_prompt',
          number: this.shopInfo.mobile
        })
      },
      addCart(){
        this.$http.post('/shop/cart/add',{goodsId:this.id,goodsNum:1,type:'add'}).then(res => {
            if(res.code==0){
                this.$vux.toast.text(res.msg);
            }else{
                this.$vux.toast.text(res.msg);
            }
        })
     }
  }
}
</script>

<style lang="less" scoped>
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
            span:first-child{
                color:#666666;
            }
            span:last-child{
                color:#666666;
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
                max-width:100%;
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
        .small-text{
            font-size:0.2rem;
            color:#666666;
            width:3rem;
        }
        .white-text{
            color:white;
        }
        .cart{
            width:2.3rem;
            height:1rem;
            line-height: 1rem;
            background: linear-gradient(to right,#FFCB00, #FF9702);
        }
        .bynow{
            width:2.3rem;
            height:1rem;
            line-height: 1rem;
            background: linear-gradient(to right,#F9574D, #EE1A23);
        }
        img{
            width:0.35rem;
            height:0.35rem;
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
