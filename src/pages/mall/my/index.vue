<template>
    <div style="background:rgb(245,245,245);width:100%;height:100%" id="my" v-if="showInfo">
      <div class="mescrollBox">
        <div id="setss" @click="jumpTo('/info')"></div>
        <mescroll-vue ref="mescroll" :down="mescrollDown" @init="mescrollInit">
          <div class="my-head" >
            <div class="box" >
              <div class="box-two" ref="boxtt">
                <div class="headimg"><img :src="data.avatar || './static/libs/img/touxiang.png'" alt=""></div>
                <div>
                  <div class="nick">
                    <div>{{data.nickname || '未设置'}}</div>
                    <img v-if="authentication == 1" src="../../../assets/img/my/authenticationOk.png" alt="">
                    <img v-if="authentication == 0" @click="goauthentication" src="../../../assets/img/my/authenticationNo.png" alt="">
                  </div>
                  <div>
                    <span class="s-1">邀请码: {{data.invite_code}}</span>
                    <span class="s-1" @click="onCopy(data.invite_code)">复制 <img class="icon-q" align="absmiddle" src="../../../assets/img/my/fuzhin.png" alt=""></span>
                  </div>
                </div>
              <div class="identity"> <img class="icon-q" align="absmiddle" src="../../../assets/img/my/hehuorenn.png" alt=""> {{center.lv}}</div>
              </div>
            </div>
          </div>
          <div class="order" style="margin-top:0.7rem;">
                <div class="order-title">
                  <p>我的卖单</p>
                  <span  @click="handSale(0)">查看全部 <img src="../../../assets/mall/img/my/right.png" alt=""></span>
                </div>
            <div class="order-list1">
              <div class="list-item1" @click="handSale(1)">
                <img src="../../../assets/img/my/daizhifu1.png" alt="">
                <span v-show="SecondOrderNum.num1 > 0">{{SecondOrderNum.num1}}</span>
                <p>待支付</p>
              </div>
              <div class="list-item1" @click="handSale(2)">
                <img src="../../../assets/img/my/daifahuo1.png" alt="">
                <span v-show="SecondOrderNum.num2 > 0">{{SecondOrderNum.num2}}</span>
                <p>待发货</p>
              </div>
              <div class="list-item1" @click="handSale(3)">
                <img src="../../../assets/img/my/daishouhuo1.png" alt="">
                <span v-show="SecondOrderNum.num3 > 0">{{SecondOrderNum.num3}}</span>
                <p>待收货</p>
              </div>
              <div class="list-item1" @click="handSale(4)">
                <img src="../../../assets/img/my/yiwancheng1.png" alt="">
                <span v-show="SecondOrderNum.num4 > 0">{{SecondOrderNum.num4}}</span>
                <p>已完成</p>
              </div>
              <div class="list-item1" @click="jumpTo('/orderLIstKey')">
                <img src="../../../assets/img/my/keyorder.png" alt="">
                <!-- <span></span> -->
                <p>金钥匙订单</p>
              </div>
            </div>
          </div>
          <div class="order" style="margin-top:0.7rem;">
                <div class="order-title">
                  <p>我的买单</p>
                  <span  @click="handDetail(0)">查看全部 <img src="../../../assets/mall/img/my/right.png" alt=""></span>
                </div>
            <div class="order-list1">
              <div class="list-item1" @click="handDetail(1)">
                <img src="../../../assets/img/my/daizhifu2.png" alt="">
                <span v-show="orderNum.num1 > 0">{{orderNum.num1}}</span>
                <p>待支付</p>
              </div>
              <div class="list-item1" @click="handDetail(2)">
                <img src="../../../assets/img/my/daifahuo2.png" alt="">
                <span v-show="orderNum.num2 > 0">{{orderNum.num2}}</span>
                <p>待发货</p>
              </div>
              <div class="list-item1" @click="handDetail(3)">
                <img src="../../../assets/img/my/daishouhuo2.png" alt="">
                <span v-show="orderNum.num3 > 0">{{orderNum.num3}}</span>
                <p>待收货</p>
              </div>
              <div class="list-item1" @click="handDetail(4)">
                <img src="../../../assets/img/my/yiwancheng2.png" alt="">
                <span v-show="orderNum.num4 > 0">{{orderNum.num4}}</span>
                <p>已完成</p>
              </div>
            </div>
          </div>
          <div>
            <div class="nine">
              <p>常用功能</p>
              <ul>
                <li @click="jumpTo('/olimon')">
                  <div>
                    <img src="../../../assets/img/my/qianbao.png" alt="">
                  </div>
                  <span>我的钱包</span>
                </li>
                <li @click="jumpTo('/goodLuck')">
                  <div>
                    <img src="../../../assets/img/my/goodLuck.png" alt="">
                  </div>
                  <span>幸运转盘</span>
                </li>
                <li @click="jumpTo('/olifen')">
                  <div>
                    <img src="../../../assets/img/my/jifen.png" alt="">
                  </div>
                  <span>我的积分</span>
                </li>
                <li @click="jumpTo('/team')">
                  <div>
                    <img src="../../../assets/img/my/tuandui.png" alt="">
                  </div>
                  <span>我的团队</span>
                </li>
                <li @click="jumpTo('/invite')">
                  <div>
                    <img src="../../../assets/img/my/invite.png" alt="">
                  </div>
                  <span>邀请用户</span>
                </li>
                <li @click="jumpTo('/mall/applyShop')" v-if="info.isShop==0">
                  <div>
                    <img src="../../../assets/img/oli/oliup.png" alt="">
                  </div>
                  <span >申请商家</span>
                </li>
                <li @click="jumpTo('/mall/buy_order')">
                  <div>
                    <img src="../../../assets/img/oli/olined.png" alt="">
                  </div>
                  <span>二手买单</span>
                </li>
                <li @click="jumpTo({name:'my_coupon'})">
                  <div>
                    <img src="../../../assets/mall/img/my/tubiao/9.png" alt="">
                  </div>
                  <span>优惠券</span>
                </li>
                <li @click="jumpTo({name:'mallShopCenter'})" v-if="info.isShop!=='0'">
                  <div>
                    <img src="../../../assets/mall/img/my/tubiao/4.png" alt="">
                  </div>
                 <!-- <span>申请商家</span>-->
                   <span >{{shopInfo.title || '商家中心'}}</span> 
                </li>
                <li @click="jumpTo({name: 'mallProductMan'})" >
                  <div>
                    <img src="../../../assets/img/my/fabu1.png" alt="">
                  </div>
                  <span>发布管理</span>
                </li>
              </ul>
            </div>
          </div>
          <!-- 油券追加添加 -->
        </mescroll-vue>
      </div>
      <!-- <img src="../../../assets/mall/shouye.png" v-if="$store.state.global.firstNav === '/home'" alt="" class="homeBack" @click="jumpTo({name:'mallHome'})"> -->
    </div>
</template>


<script>
import './my.less'
import { swiper, swiperSlide } from 'vue-awesome-swiper'
import { Marquee, MarqueeItem } from 'vux'
import * as apiHttp from '../../../api/index'
import * as utils from '../../../utils'
import MescrollVue from 'mescroll.js/mescroll.vue'

import EditSlide from '../../../components/EditSlide'
import EditNav from '../../../components/EditNav'
import EditNavSlide from '../../../components/EditNavSlide'
import EditHeader from '../../../components/EditHeader'
import EditBlank from '../../../components/EditBlank'
import EditRichText from '../../../components/EditRichText'
import EditText from '../../../components/EditText'
import EditImg from '../../../components/EditImg'
import EditCube from '../../../components/EditCube'
import EditGoodsSlide from '../../../components/EditGoodsSlide'
import EditMallNotice from '../../../components/EditMallNotice'
import EditNavGroup from '../../../components/EditNavGroup'
import EditOrder from '../../../components/EditOrder'
export default {
  name: 'index',
  components: {
    Marquee,
    MarqueeItem,
    swiper,
    swiperSlide,
    MescrollVue,
    EditSlide,
    EditNav,
    EditNavSlide,
    EditHeader,
    EditBlank,
    EditRichText,
    EditImg,
    EditCube,
    EditText,
    EditGoodsSlide,
    EditMallNotice,
    EditNavGroup,
    EditOrder
  },
  inject: ['reload'],
  data () {
    return {
      swiperOption4: {
        direction: 'vertical',
        autoplay: {
          delay: 3000,
          waitForTransition: true
        }
      },
      data: this.$store.state.user.userInfo,
      info: {},
      userInfo: {},
      shopInfo: {},
      expressList: {},
      finish: false,
      orderNum: '',
      SecondOrderNum:'',
      titleInfo: {},
      isDisUser: {},
      disUserInfo: {},
      showInfo: false,
      mescroll: null,
      center:'',
      authentication: '',
      mescrollDown: {
        auto: false,
        callback: this.refresh
      }
    }
  },
  mounted () {
    this.getMyInfo()
    this.getDesign()
  },
  created() {
    this.getInfo ()
  },
  methods: {
    aaa() {
      this.jumpTo('/info')
    },
    goauthentication () {
      this.jumpTo('/authentication')
    },
    // 复制
    onCopy (orderSn) {
      let that = this
      let clipBoard = api.require('clipBoard')
      clipBoard.set({
        value: orderSn
      }, function (ret, err) {
        if (ret) {
          that.$vux.toast.text('复制成功')
        } else {
          that.$vux.toast.text('请重试')
        }
      })
    },
    getDesign () {
      this.$http.post('/amoy/app/design', {
        type: 2,
        site_type: 1,
        version: api.appVersion,
        device: (api.systemType === 'android') ? 1 : 2
      }, true, true).then(res => {
        if (res.code === 0) {
          if (res.data.content) {
            this.$store.commit('setMallMy', JSON.parse(res.data.content))
          }
        }
      })
    },
    getInfo () {
      if (this.$store.state.user.token) {
        this.$http.post('/amoy/user/user-info', {}).then(res => {
          if (res.code === 0) {
            this.userInfo = res.data
            this.center = res.data;
            this.authentication = res.data.authentication
            this.$store.commit('setUserInfo', res.data)
            console.log("authentication",this.userInfo.authentication)
          }
        }).catch((e) => {
          this.mescroll.endErr()
        })
      }
    },
    eventFun () {
      this.$router.go(-1)
    },
    getMyInfo () {   // 绑定数据的接口
      apiHttp.personInfo().then(res => {
        if (res.code === 0) {
          this.showInfo = true
          this.info = res.data
          this.userInfo = res.data.userInfo
          this.shopInfo = res.data.shopInfo
          this.expressList = res.data.expressList
          this.orderNum = res.data.orderNum
          this.SecondOrderNum = res.data.SecondOrderNum
          this.titleInfo = res.data.titleInfo.alias
          utils.storage.set('titleInfo', this.titleInfo)
          this.isDisUser = res.data.isDisUser
          this.disUserInfo = res.data.disUserInfo
        }
        this.$nextTick(() => {
          this.mescroll.endSuccess(this.info.length, false)
        })
      })
    },
    mescrollInit (mescroll) {
      this.mescroll = mescroll
    },
    refresh (done) {
      this.getDesign()
      this.finish = false
      this.getMyInfo()
    },
    handDetail (key) {
      utils.storage.set('orderId', key)
      this.$router.push({
        name: 'mallFixing_order',
        query: {
          id: key
        }
      })
    },
    handSale(key){
      utils.storage.set('orderId', key)
      this.$router.push({
        name: 'mallSale_order',
        query: {
          id: key
        }
      })
    },
    applyDe () {
      this.$store.commit('logout')
      this.reload()
      this.$router.replace('/wechatLogin')
    }
  }
}
</script>

<style lang="less">
  @import '../../../assets/mall/style/_Variables';
  .nick{
    display: flex;
    align-items: center;
    font-size: .32rem;
    font-weight:bold;
    color:rgba(51,51,51,1);
    >div{
      max-width: 2rem;
      overflow: hidden;
      text-overflow:ellipsis;
      white-space: nowrap;
    }
    img{
      margin-left: .13rem;
      width: 1.2rem !important;
      height: .43rem !important;
    }
  }
  #setss {
    position: absolute;
    top:.5rem;
    right: .3rem;
    width: .5rem;
    height: .5rem;
    border-radius:50%;
    background: url("../../../assets/img/oli/oliset.png") no-repeat center center;
    background-size: 100% 100%; 
    z-index:999
  }
  .box-two {
    color:rgba(51,51,51,1) !important;
  }
  #my{
    text-align: center;
    .homeBack{
      position: fixed;
      right: 0.34rem;
      bottom: 146px;
      width: 36px;
      height: 36px;
      z-index: 9990;
    }
    .mescrollBox {
      width: 100%;
      position: absolute;
      top: 0;
      bottom: 43px;
      height: auto !important;
      .my-foot{
        margin-top: .2rem;
        display: flex;
        justify-content: flex-start;
        align-items: center;
        padding: .2rem .3rem;
        background: #ffffff;
        flex-direction: row;
        flex-wrap: wrap;
        div{
          display: flex;
          flex-direction: column;
          justify-content: center;
          height: 1.5rem;
          align-items: center;
          width: 25%;
          img{
            height: 0.45rem;
            width: 0.45rem;
          }
          p{
            margin-top: .15rem;
            font-size: @font-nomal;
            color: @font-main-color;
          }
        }
      }
    }
  }
  .nine {
    width: 95%;
    margin: .2rem auto;
    padding-bottom: .5rem;
    border-bottom: 0.5px solid #e7e7e7;
    background-color: #fff;
    border-radius:10px; 
    overflow: hidden;
    p {
      display: block;
      width: 100%;
      height: .5rem;
      text-align: left;
      padding-left: .2rem;
      padding-bottom: .3rem;
      font-size: .33rem;
      font-weight: 600;
      margin: .4rem 0 ;
      border-bottom:.5px solid #EEEEEE;
      line-height: .5rem; 
    }
  ul {
    display: flex;
    // justify-content: space-around;
    flex-wrap: wrap;
    li {
      width: 25%;
      list-style: none;
      text-align: center;
      font-size: .25rem;
      font-weight:400;
      color:rgba(51,51,51,1);
      line-height: .5rem;
      margin-bottom:.1rem; 
      div {
        width: .5rem;
        height: .5rem;
        margin: .15rem auto;
        img {
          width: 100%;
        }
      }
    }
  }
}
</style>
