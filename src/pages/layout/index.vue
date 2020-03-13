<template>
  <view-box ref="viewBox" id="layout" :body-padding-top="$route.meta.isShowHead || $route.meta.isShowSearch ? '.88rem' : '0'" :body-padding-bottom="$route.meta.isShowTab ? h : '0'">
    <x-header
      v-show="$route.meta.isShowHead"
      slot="header"
      :left-options="{showBack: false}"
      style="width:100%;position:absolute;left:0;top:0;z-index:100;"
    >
      <a v-show="!$route.meta.goBack" slot="left" @click="$route.meta.address ? jumpTo($route.meta.address) : goBack()" class="backBox"><i class="iconfont icon-back"></i></a>
      {{$route.query.title ? $route.query.title : $route.meta.title}}
      <a slot="right" v-show="$route.meta.ttt">
        <i class="iconfont" @click="jumpTo($route.meta.zzz)">{{$route.meta.vv}}</i>
      </a>
    </x-header>
      <transition :name='transitionName'>
        <keep-alive  include="block,home,otherclassify,dayingList,daying,dayingShop,classify,sort,block,my,my1,my2,my3,binding,invite,pullNew,pullNewRule,sharePoster,materials,materials2,circle,list,select,super,results,personSpeak,personArticle,mallBusiness,wechatLogin,newIndex">
        <router-view></router-view>
        </keep-alive>
      </transition>
    <div class="tabbarBox" id="tabbarBox"  v-show="$route.meta.isShowTab" :style="{paddingBottom: bot + 'px',background:footer.bgColor}">
      <template v-for="(item, key) of footer.items">
        <div class="upTab" :key="key" @click="jumpLink(item.link)" v-if="item.style && item.style === '1'">
          <div  class="tabbarIcon" >
            <img :src="item.img" style="" alt="">
          </div>
          <p class="tabbarName" style="padding-top:0rem">发布</p>
        </div>
        <div class="tabbar"  :key="key"  @click="jumpLink(item.link)" v-else>
          <div class="tabbarIcon"  v-if="item.link && $route.path === item.link.url">
            <img :src="item.activeImg" alt="" >
          </div>
          <div  class="tabbarIcon" v-else>
            <img :src="item.img" alt="">
          </div>
          <p class="tabbarName" :style="{color: item.activeColor}" v-if="item.link && $route.path === item.link">{{item.text}}</p>
          <p class="tabbarName" :style="{color: item.color}" v-else>{{item.text}}</p>
        </div>
      </template>
    </div>
    <div class="tabbarBox" id="tabbar1" ref="tabbar" v-if="$store.state.global.mallFooter" v-show="$route.meta.isShopShowTab && ($store.state.global.firstNav !== '/home' || $isMallShop)" :style="{paddingBottom: bot + 'px',background:$store.state.global.mallFooter.bgColor}">
      <template v-for="(item, key) of $store.state.global.mallFooter.items">
        <div class="upTab" :key="key" @click="jumpLink(item.link)" v-if="item.style && item.style === '1'">
          <div  class="tabbarIcon">
            <img :src="item.img"  alt="">
          </div>
        </div>
        <div class="tabbar"  :key="key"  @click="jumpLink(item.link)" v-else>
          <div class="tabbarIcon"  v-if="item.link && $route.path === item.link.url">
            <img :src="item.activeImg" alt="" >
          </div>
          <div  class="tabbarIcon" v-else>
            <img :src="item.img" alt="">
          </div>
          
          <p class="tabbarName" :style="{color: item.activeColor}" v-if="item.link && $route.path === item.link">{{item.text}}</p>
          <p class="tabbarName" :style="{color: item.color}" v-else>{{item.text}}</p>
        </div>
      </template>
    </div>
  </view-box>
</template>

<script>
import '../../assets/js/api'
import { ViewBox, Tabbar, TabbarItem, XHeader } from 'vux'
export default {
  name: 'index',
  data () {
    return {
      h: '49px',
      transitionName: '',
      pageName: ['home', 'my'],
      bot: '49px',
      footer:{
          "bgColor":"rgba(255, 255, 255, 1)",
          "num":"5",
          "items":{
              "F0000000000001":{
                  "img":'./static/img/home/home.png',
                  "activeImg":'./static/img/home/ahome.png',
                  "text":"首页",
                  "color":"rgba(153,153,153,1)",
                  "activeColor":"rgba(252,51,87,1)",
                  "link":{
                      "url":"/home",
                      "label":"首页",
                      "type":0
                  }
              },
              "F0000000000002":{
                  "img":'./static/img/home/guaran.png',
                  "activeImg":'./static/img/home/aguaran.png',
                  "text":"担保",
                  "color":"rgba(153,153,153,1)",
                  "activeColor":"rgba(252,51,87,1)",
                  "link":{
                      "url":"/mall/underShop",
                      "label":"担保",
                      "type":0
                  }
              },
              "F0000000000003":{
                  "img":'./static/img/home/release.png',
                  "activeImg":'./static/img/home/release.png',
                  "text":"发行",
                  "color":"rgba(153,153,153,1)",
                  "activeColor":"rgba(252,51,87,1)",
                  "link":{
                      "url":"/product",
                      "label":"发行",
                      "type":0
                  },
                  "style": "1"
              },
              "F0000000000004":{
                  "img":'./static/img/home/proper.png',
                  "activeImg":'./static/img/home/aproper.png',
                  "text":"自行",
                  "color":"rgba(153,153,153,1)",
                  "activeColor":"rgba(252,51,87,1)",
                  "link":{
                      "url":"/mall/tradSale",
                      "label":"自行",
                      "type":0
                  }
              },
              "F0000000000005":{
                  "img":'./static/img/home/my.png',
                  "activeImg":'./static/img/home/amy.png',
                  "text":"我的",
                  "color":"rgba(153,153,153,1)",
                  "activeColor":"rgba(252,51,87,1)",
                  "link":{
                      "url":"/mall/my",
                      "label":"我的",
                      "type":0
                  }
              }
          }
      }
    }
  },
  components: {
    ViewBox,
    Tabbar,
    TabbarItem,
    XHeader
  },
  methods: {
  },
  created () {
    
  },
  mounted () {
    document.getElementById('tabbarBox').style.paddingBottom = api.safeArea.bottom + 'px'
    document.getElementById('tabbar1').style.paddingBottom = api.safeArea.bottom + 'px'
  },
  // 此段添加内容
  watch: {
    '$route' (to, from) {
      if (from.meta.routerZindex > to.meta.routerZindex) {
        this.transitionName = 'slide-right'
      } else if (from.meta.routerZindex < to.meta.routerZindex) {
        this.transitionName = 'slide-left'
      } else {
        this.transitionName = 'slide-none'
      }
    }
  }
}
</script>

<style scoped lang="less">
  #tabbar{
    height: 49px;
  }
  #tabbar1{
    height: 49px;
  }
  .router-fade-enter-active, .router-fade-leave-active {
    position: absolute;
    width: 100%;
    transition: opacity .3s;
  }
  .router-fade-enter, .router-fade-leave-active {
    opacity: 0;
  }
  .weui-tabbar{
    background: #fff;
  }
  .weui-tabbar:before{
    border-top: 1px solid #e0dee6;
  }
</style>
<style lang="less">
  #layout{
    .weui-tabbar__item{
      padding-top: 7px;
    }
    .vux-header{
      .vux-header-left{
        left: 0;
        top: 0;
        .backBox{
          display: block;
          height: 0.88rem;
          line-height: .88rem;
          padding: 0 .2rem;
        }
      }
    }
    .tabbarBox{
      z-index: 2000;
      box-shadow: 0 4px 8px #525252;
      position: absolute;
      bottom: 0;
      left: 0;
      height: 49px;
      padding-top: 5px;
      box-sizing: content-box;
      width: 100%;
      display: flex;
      .tabbar{
        flex: 1;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        .tabbarIcon{
          width: 23px;
          height: 23px;
          display: inline-block;
          margin-bottom: 1px;
          img{
            width: 23px;
            height: 23px;
            display: inline-block;
          }
        }
        .tabbarName{
          font-size: 12px;
          color: #999;
          line-height: 1.8;
          text-align: center;
        }
      }
      .upTab{
        flex: 1;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        .tabbarIcon{
          background: #fff;
          margin-top: -0.65rem;
          width: 1.2rem;
          height: 1.2rem;
          border-radius: 50%;
        }
      }
    }
  }
</style>
