<template>
  <div id="vue-app" v-cloak>
    <x-header
        slot="header"
        :left-options="{showBack: false}"
      >
        <a @click="goBack()" slot="left" class="backBox"><i class="iconfont icon-back"></i></a>
        幸运大转盘
        <p style="position: absolute;right: .3rem;top: 0;color: rgba(153,153,153,1);font-size:.28rem;" @click="jumpTo('/luckList')">抽奖记录</p>
    </x-header>
    <div class="core_box">
      <div style="height:4.7rem;"></div>
      <div class="box-lottery">
        <div class="lottery-wrap" id='lottery' :style="{transform:rotateAngle,transition:tranInof}">
          <span :class="'lottery-span'+(index+1)" v-for="(item, index) in list" :key = "index" >
            <i>{{item.title}}</i>
            <img :src="item.image" :alt="index">
          </span>
        </div>
        <a class="lottery-btn"><i></i></a>
        <div class="lottery_core"  @click="lottery()">
          <div>开始</div>
          <div>(积分{{consume}})</div>
        </div>
      </div>
      <div class="box_bot">
        <div class="luck_tit"><img src="./img/luck1.png" alt=""></div>
        <div class="luck_box">
          <div class="luck_core">
            <div v-for="(item,index) in list" :key="index">
              <span v-if="index== '0'">一等奖：</span>
              <span v-if="index== '1'">二等奖：</span>
              <span v-if="index== '2'">三等奖：</span>
              <span v-if="index== '3'">四等奖：</span>
              <span v-if="index== '4'">五等奖：</span>
              <span v-if="index== '5'">六等奖：</span>
              <span v-if="index== '6'">七等奖：</span>
              <span v-if="index== '7'">八等奖：</span>
              {{item.title}}
              </div>
          </div>
        </div>
        <div class="luck_tit"><img src="./img/luck2.png" alt=""></div>
        <div class="luck_box">
          <div class="luck_core2" v-if="luck.length!='0'">
            <div  v-for="(item,index) in luck" :key="index">
              <span v-if="item.turntable_id== '1'">一等奖：</span>
              <span v-if="item.turntable_id== '2'">二等奖：</span>
              <span v-if="item.turntable_id== '3'">三等奖：</span>
              <span v-if="item.turntable_id== '4'">四等奖：</span>
              <span v-if="item.turntable_id== '5'">五等奖：</span>
              <span v-if="item.turntable_id== '6'">六等奖：</span>
              <span v-if="item.turntable_id== '7'">七等奖：</span>
              <span v-if="item.turntable_id== '8'">八等奖：</span>
              <span>{{item.turn_reward}}</span>
              <span>{{item.mobile}}</span>
            </div>
          </div>  
          <div class="luck_core2 luck_coreKong" v-else>
            <img src="./../../../static/img/auth/kong.png" alt="">
            <div>暂无相关数据~</div>  
          </div>
        </div>
        <div style="height:1rem;"></div>
      </div>
    </div>
    <!-- 弹窗 -->
     <div>
      <confirm
      v-model="upgradeWin"
      :show-cancel-button="false"
      :title="yes"
      @on-confirm="onConfirm">
        <!-- <p style="text-align:center;">请去升级会员</p> -->
      </confirm>
    </div>
  </div>
</template>

<script>
import { Confirm, Tab, TabItem, Popup } from 'vux'
import * as apiHttp from '../../api/index'
import {formatDate} from '../../plugin/data'
import MescrollVue from 'mescroll.js/mescroll.vue'

export default {
  name: 'goodLuck',
  components: {
    Confirm,
    MescrollVue,
    Tab,
    TabItem,
    Popup
  },
  data () {
    return {
        rotateNumber : '', // 旋转的圈数
        rotateAngle : '', // 赋值旋转角度后的CSS3属性
        tranInof : 'transform 3s ease-in-out',
        consume: '', //抽奖所需积分
        list: [],
        luck:[],
        credit1: '', // 积分
        upgradeWin: false, // 弹窗
        yes: '',
        repetition: true 
    }
  },
  mounted () {
    this.getstyle()
    this.getjifen()
  },
  methods: {
    lottery () {
      if (Number(this.credit1) >= Number(this.consume)) {
        if (this.repetition == true) {
          this.repetition = false
          this.$http.post('/mall/gold/luck-draw', {}).then(res => {
            if (res.code == 0){
              this.tranInof = 'transform 3s ease-in-out'
              this.rotateNumber = 360*5 + (360-(22.5+(res.data.id-1)*(360/this.list.length)))
              this.rotateAngle = 'rotate(' + this.rotateNumber + 'deg)'
              this.yes = res.data.yes
              setTimeout(()=>{
                this.upgradeWin = true
                this.repetition = true
              },3500)
            }
          })
        } else {
          this.$vux.toast.text('请不要重复点击')
        }
      } else {
        this.$vux.toast.text('积分不足！')
      }
    },
    onConfirm () {
      this.upgradeWin = false
      this.rotateNumber = ''
      this.rotateAngle = ''
      this.tranInof = 'transform .2s ease-in-out'
    },
    getjifen () {
      this.$http.post('/amoy/user/user-info', {}).then(res => {
        if (res.code == 0) {
          this.credit1 = res.data.credit1
          console.log(this.credit1)
        }
      })
    },
    getstyle () { //抽奖配置
      this.$http.post('/mall/gold/turn-config', {}).then(res => {
        if (res.code == 0){
          this.consume = res.data.consume
          this.list = res.data.list
          this.luck = res.data.luck
        }
      })
    }
  }
}

</script>
<style lang="less" scoped>
.core_box{
  width: 100%;
  height: 100%;
  background: url(img/bg.png) 0 0 no-repeat;
  background-size: 100% 13.34rem;
  overflow: hidden;
}
#vue-app{
  width: 100%;
}
#lottery {
  // transform: rotate(-45deg);
}
.box-lottery {
  position: relative;
  margin: 0 auto 0;
  width: 7.5rem;
  height: 7.5rem;
}
.lottery-wrap {
  position: relative;
  width: 100%;
  height: 100%;
  background: url('img/bg_lottery_f.png') center no-repeat;
  background-size: 6.5rem 6.5rem;
}
.lottery-wrap img {
  width: .7rem;
  height: auto;
}
.lottery-wrap span {
  box-sizing: border-box;
  position: absolute;
  top: 0;
  left: 35.5%;
  width: 2.14rem;
  height: 50%;
  color: #fff;
  -webkit-transform-origin: 50% 100%;
  -moz-transform-origin: 50% 100%;
  -ms-transform-origin: 50% 100%;
  transform-origin: 50% 100%;
  text-align: center;
}
.lottery-wrap span.lottery-span1 {
  -webkit-transform: rotate(22.5deg);
  -moz-transform: rotate(22.5deg);
  -ms-transform: rotate(22.5deg);
  transform: rotate(22.5deg);
}
.lottery-wrap span.lottery-span2 {
  -webkit-transform: rotate(67.5deg);
  -moz-transform: rotate(67.5deg);
  -ms-transform: rotate(67.5deg);
  transform: rotate(67.5deg);
}
.lottery-wrap span.lottery-span3 {
  -webkit-transform: rotate(112.5deg);
  -moz-transform: rotate(112.5deg);
  -ms-transform: rotate(112.5deg);
  transform: rotate(112.5deg);
}
.lottery-wrap span.lottery-span4 {
  -webkit-transform: rotate(157.5deg);
  -moz-transform: rotate(157.5deg);
  -ms-transform: rotate(157.5deg);
  transform: rotate(157.5deg);
}
.lottery-wrap span.lottery-span5 {
  -webkit-transform: rotate(202.5deg);
  -moz-transform: rotate(202.5deg);
  -ms-transform: rotate(202.5deg);
  transform: rotate(202.5deg);
}
.lottery-wrap span.lottery-span6 {
  -webkit-transform: rotate(247.5deg);
  -moz-transform: rotate(247.5deg);
  -ms-transform: rotate(247.5deg);
  transform: rotate(247.5deg);
}
.lottery-wrap span.lottery-span7 {
  -webkit-transform: rotate(292.5deg);
  -moz-transform: rotate(292.5deg);
  -ms-transform: rotate(292.5deg);
  transform: rotate(292.5deg);
}
.lottery-wrap span.lottery-span8 {
  -webkit-transform: rotate(337.5deg);
  -moz-transform: rotate(337.5deg);
  -ms-transform: rotate(337.5deg);
  transform: rotate(337.5deg);
}
.lottery-wrap i {
  display: block;
  width: 100%;
  font-style: normal;
  font-size: .25rem;
  font-weight: bold;
  color: #FF311E;
  margin: 1.3rem  0 .12rem;
}
.lottery-btn {
  position: absolute;
  left: 50%;
  top: 50%;
  text-indent: -999em;
  z-index: 11;
  -webkit-transform: translate(-50%, -50%);
  -moz-transform: translate(-50%, -50%);
  -ms-transform: translate(-50%, -50%);
  transform: translate(-50%, -50%);
  width: 1.61rem;
  height: 1.61rem;
}
.lottery-btn i {
  position: absolute;
  top: -.3rem;
  left: 47%;
  margin-left: -.2rem;
  width: .41rem;
  height: .39rem;
  background: url('img/icon_point.png') center no-repeat;
  background-size: .45rem;
  -webkit-transform-origin: 50% 1.025rem;
  -moz-transform-origin: 50% 1.025rem;
  -ms-transform-origin: 50% 1.025rem;
  transform-origin: 50% 1.025rem;
}
.lottery-btn:after {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: url('img/btn_lottery.png') center no-repeat;
  background-size: 1.61rem 1.61rem;
}
.lottery_core {
  position: absolute;
  left: 50%;
  top: 50%;
  // text-indent: -999em;
  z-index: 20;
  -webkit-transform: translate(-50%, -50%);
  -moz-transform: translate(-50%, -50%);
  -ms-transform: translate(-50%, -50%);
  transform: translate(-50%, -50%);
  width: 1.61rem;
  height: 1.61rem;
  text-align: center;
  color:rgba(254,254,254,1);
  >div:nth-child(1){
    font-size: .36rem;
    font-weight:800;
    margin-top: .2rem;
  }
   >div:nth-child(2){
    font-size: .2rem;
    font-weight:500;
  }
}
.box_bot{
  background:linear-gradient(0deg,rgba(255,148,77,1),rgba(227,23,23,1));
  // height: 3rem;
  .luck_tit{
    width: 100%;
    text-align: center;
    img{
      width: 2.95rem;
      height: .4rem;
      margin: .3rem 0;
    }
  }
  .luck_box{
    padding: 0 .3rem;
    font-size: .28rem;
    font-weight:bold;
    .luck_core{
    height: 3.1rem;
    background:rgba(255,255,255,1);
    border: .1rem solid rgba(255,192,94,1);
    box-shadow: 0 .1rem .2rem 0 rgba(185,21,14,0.3);
    border-radius: .24rem;
    box-sizing: border-box;
    display: flex;
    flex-wrap: wrap;
      >div{
        width: 50%;
        display: flex;
        align-items: center;
        span:nth-child(1){
          color: #E41717;
          margin-left: .2rem;
        }
      }
    }
    .luck_core2{
      height: 6.5rem;
      background:rgba(255,255,255,1);
      border: .1rem solid rgba(255,192,94,1);
      box-shadow: 0 .1rem .2rem 0 rgba(185,21,14,0.3);
      border-radius: .24rem;
      box-sizing: border-box;
      padding: .3rem .4rem;
      overflow-y: scroll;
      >div{
        height: .97rem;
        display: flex;
        justify-content: space-between;
        align-items: center;
        span:nth-child(1){
          color: #E41717;
        }
        span:nth-child(2){
          color:rgba(51,51,51,1);
          font-size: .28rem;
          font-weight:500;
        }
        span:nth-child(3){
          color:rgba(153,153,153,1);
          font-size: .28rem;
          font-weight:500;
        }
      }
    }
    .luck_coreKong{
     text-align:center;
     >img{
       margin-top: .5rem;
     }
     >div{
       color:#e2e2e2;
       justify-content:center;
     }
    }
  }
}
</style>
