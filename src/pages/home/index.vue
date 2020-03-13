<template>
  <div id="home"  ref="home">
    <div class="dialog" v-if="showMore" @click="showMore = false"></div>
    <transition name="showlist">
      <div class="moreBox" v-if="showMore"   :style="{top: headBoxH + 'rem'}">
        <div class="t"><span>全部分类</span><i class="iconfont icon-close" @click="showMore = false,categoryIndex=0"></i></div>
        <div class="classify">
          <div class="classifyBox" v-for="(item, index) of secondList" :key="item.id" @click="jumpTo({name:'mallProductList',query:{id:item.id,title:item.title}})">
            <img v-lazy="item.thumb" alt="" >
            <span>{{item.title}}</span>
          </div>
        </div>
      </div>
    </transition>
    <div class="swiperBg" :style="{paddingTop: statusH + 'rem',background: swiperBg}">
      <img src="../../assets/img/home/swiperBg.png" alt="">
      <div class=""></div>
    </div>
    <div  class="headBox" ref="headBox" :style="{paddingTop: statusH + 'rem',background: 'linear-gradient(#01C2C7,#01C2C7)'}">
      <div class="header" >
        <img :src="$store.state.global.search.leftIcon" alt="" class="signIn" @click="jumpLink($store.state.global.search.leftLink)" v-if="$store.state.global.search.leftIcon">
        <div class="searchBox" @click="jumpTo({name:'mallSearch',query:{type:'shop'}})" :style="{background: $store.state.global.search.searchBgColor}">
          <img :src="$store.state.global.search.searchIcon" alt="">
          <p :style="{color:$store.state.global.search.color}">{{$store.state.global.search.searchPlaceholder}}</p>
        </div>
        <img  :src="$store.state.global.search.rightIcon" v-if="$store.state.global.search.rightIcon"  alt="" class="shopImg" @click="jumpLink($store.state.global.search.rightLink)">
      </div>
     <div class="navBox homeNav"   style="background:#01C2C7">
        <div class="tabBox" ref="tabBox" v-if="category">
          <div class="tabIndex" >
            <div @click="showMore=false,categoryIndex=0"  :class="{'home-active': categoryIndex === 0}" v-if="categoryIndex === 0" >精选</div>
            <div @click="showMore=false,categoryIndex=0"  :class="{'home-active': categoryIndex === 0}" v-else >精选</div>
            <template v-for="(itemNav, index) of category">
              <div  :key="itemNav.id" @click="change(itemNav.id, index + 1)" :class="{'home-active': categoryIndex === (index + 1)}" v-if="categoryIndex === (index + 1)" >{{itemNav.title}}</div>
              <div  :key="itemNav.id" @click="change(itemNav.id, index + 1)" :class="{'home-active': categoryIndex === (index + 1)}" v-else>{{itemNav.title}}</div>
            </template>
          </div>
          <div class="more" @click="jumpTo('/mall/shopProductList')">
            <i class="iconfont icon-sort" ></i>
          </div>
        </div>
      </div>
    </div>
    <!--分类-->
    <div class="list" :class="{over: showMore}" ref="list" :style="{top: 2.3 + 'rem'}">
      <mescroll-vue ref="mescroll" :down="mescrollDown" :up="mescrollUp" @init="mescrollInit" :style="{overflowY: showMore ? 'hidden' : 'auto'}">
        <!--<template v-for="item of $store.state.global.content" v-if="!cid">
          <edit-slide v-if="item.name === 'edit-slide' && slide"  :data="item.params" ref="editSlide" @swiperChange="swiperChange" :key="item.id" ></edit-slide>
        </template>-->
        <template>
          <edit-slide   :data="swiperslide"  ></edit-slide>
          <edit-nav-slide   :data="mallDisplay"   ></edit-nav-slide>
        </template>
        <div v-if="article" class="mall-poster" @click="openArticl(article[0].id)"><img style="max-width:100%;" :src="article[0].thumb" alt=""></div>
        <div class="home-box-hei"></div>
        <div class="kill-tit"  v-if="kill">
          <div>限时秒杀</div>
          <div v-show="endTime">{{count}}</div>
        </div>
        <edit-img :data="killList" v-if="kill" ></edit-img>
        <div class="home-box-hei" v-if="kill"></div>
        <template  v-if="shopGrid.length>0">
          <div class="listBox" >
            <template >
              <ul class="listItem" >
                <li v-for="(shop, j) of shopGrid" :key="j" @click="handDetail(shop.goodsId)">
                  <!--<p style="font-weight:bold" :style="{color:color==1?(color==1?'rgb('+Math.floor(Math.random() * 256)+','+Math.floor(Math.random() * 256)+','+Math.floor(Math.random() * 256)+')':val.iconcolor):'#ccc'}">{{shop.title}}</p>-->
                  <div class="logoBox" :style="{ 'background': 'url(' + shop.thumb + ') no-repeat center center', 'background-size': '1.5rem 1.5rem'}" ><!--<img  :src="shop.thumb" alt="">--></div>
                </li>
              </ul>
            </template>
          </div>
        </template>
        <div v-if="article" class="mall-poster" @click="openArticl(article[1].id)"><img style="max-width:100%;" :src="article[1].thumb" alt=""></div>
        <div class="home-box-hei"></div>
        <div class="home_header" v-if="shopList.length > 0">
          <img src="../../assets/mall/img/home/cainixihuan.png" alt="">
        </div>
        <list :list="shopList" ref="recommend"></list>
        <!--<div class="secondClassify" >
          <div class="classify" v-for="(item, index) of category" :key="index" @click="toList(item)">
            <img :src="item.img" alt="">
            <span>{{item.title}}</span>
          </div>
        </div>-->
     </mescroll-vue>
    </div>
  </div>
</template>

<script>
import {Tab, TabItem, Group, Cell, XHeader, Confirm, XDialog, Badge, XCircle, TransferDomDirective as TransferDom, Swiper, SwiperItem} from 'vux'
import * as utils from '../../utils'
import MescrollVue from 'mescroll.js/mescroll.vue'
import list from '../../components/list'
import GoodsItem from '../../components/GoodsList'
import EditSlide from '../../components/EditSlide'
import EditNav from '../../components/EditNav'
import EditNavSlide from '../../components/EditNavSlide'
import EditHeader from '../../components/EditHeader'
import EditBlank from '../../components/EditBlank'
import EditRichText from '../../components/EditRichText'
import EditText from '../../components/EditText'
import EditImg from '../../components/EditImg'
import EditCube from '../../components/EditCube'
import EditGoodsSlide from '../../components/EditGoodsSlide'
import EditBobao from '../../components/EditBobao'
export default {
  name: 'home',
  components: {
    Tab,
    TabItem,
    Group,
    Cell,
    XHeader,
    Confirm,
    XCircle,
    XDialog,
    Badge,
    MescrollVue,
    GoodsItem,
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
    Swiper,
    SwiperItem,
    EditBobao,
    list
  },
  directives: {
    TransferDom
  },
  data () {
    return {
      tbk: true,
      times: '',
      goodsList: false,
      slide: true,
      swiperBg: '',
      secondList:[],
      swiperOption2: {
        effect: 'coverflow',
        grabCursor: true,
        centeredSlides: true,
        loop: true,
        loopedSlides: 4,
        slidesPerView: 3,
        initialSlide: 1,
        spaceBetween: 0,
        coverflowEffect: {
          rotate: 5,
          stretch: 20,
          depth: 80,
          modifier: 3,
          slideShadows: false
        }
      },
      swiperslide:[],
      sortName: '综合',
      multiple: false,
      other: '',
      otherSwiper: '',
      classifySort: 0,
      styles: true,
      headBoxH: 0,
      statusH: '',
      kill:true,
      // text: '无所谓',
      color: '#fff',
      perCon: '0%',
      showToast1: false,
      percent: 0,
      wgtVer: '',
      size: '',
      wgtUrl: '',
      update: '',
      showToast: false,
      result: '',
      showMore: false,
      num: 6,
      imgs: [],
      icons: [],
      list: [],
      page: 1,
      sort: '',
      loading: false,
      category: '',
      categoryIndex: 0,
      cid: '',
      state: '',
      url: ['/amoy/home/goods-list', '/amoy/taobao/search'],
      type: 0,
      keyword: '',
      info: '',
      shopGrid:[],
      scrollH: false,
      scrollTopH: 0,
      mescroll: null,
      mallDisplay:'',
      mescrollDown: {
        auto: false,
        callback: this.refresh
      },
      mescrollUp: {
        user: false,
        callback: this.upCallback,
        page: {
          num: 0
        },
        scrollbar: {
          user: false
        },
        htmlNodata: '<p class="upwarp-nodata">-- END --</p>',
        htmlLoading: '<p class="upwarp-progress mescroll-rotate"></p><p class="upwarp-tip">加载中..</p>',
        noMoreSize: 5,
        toTop: {
          /*src: './static/img/auth/back_top.png',*/
          offset: 1000
        },
        empty: {
          warpId: 'empty',
          icon: './static/img/auth/kong.png',
          tip: '暂无相关数据~'
        }
      },
      dataList: [],
      lastScrollTop: 0, // 路由切换时滚动条的位置
      lastBounce: null, // 路由切换时是否禁止ios回弹
      randColor:'',
      color:1,
      shopList:[],
      killList:[],
      article:'',
      second:'',
      timer:'',
      count:'00:00:00',
      endTime:'',
    }
  },
  beforeRouteEnter (to, from, next) {
    next(vm => {
      if (vm.$store.state.global.floating && vm.$store.state.global.floating.items) {
        for (let i in vm.$store.state.global.floating.items) {
          if (vm.$store.state.global.floating.items[i].img) {
            api.openFrame({
              name: 'suspend_icon',
              url: 'widget://html/suspend_icon.html',
              rect: {
                w: 70,
                h: 70,
                marginRight: 0,
                marginBottom: 200
              },
              pageParam: {
                img: vm.$store.state.global.floating.items[i].img,
                link: vm.$store.state.global.floating.items[i].link
              }
            })
          }
        }
      }
      vm.slide = false
      vm.showMore = false
      // vm.change(0, 0)
      vm.$nextTick(() => {
        vm.slide = true
      })
      if (vm.mescroll) {
        vm.mescroll.hideTopBtn(0)
        // 滚动到之前列表的位置
        if (vm.lastScrollTop) {
          vm.mescroll.setScrollTop(vm.lastScrollTop)
          setTimeout(() => { // 需延时,因为setScrollTop内部会触发onScroll,可能会渐显回到顶部按钮
            vm.mescroll.setTopBtnFadeDuration(0) // 设置回到顶部按钮显示时无渐显动画
          }, 16)
        }
        // 恢复到之前设置的isBounce状态
        if (vm.lastBounce != null) vm.mescroll.setBounce(vm.lastBounce)
      }
    })
  },
  beforeRouteLeave (to, from, next) {
    if (window.api) {
      // 关闭悬浮按钮的frame
      api.closeFrame({
        name: 'suspend_icon'
      })
    }
    this.showMore = false
    if (this.mescroll) {
      this.lastScrollTop = this.mescroll.getScrollTop() // 记录当前滚动条的位置
      this.mescroll.hideTopBtn(0) // 隐藏回到顶部按钮,无渐隐动画
      this.lastBounce = this.mescroll.optUp.isBounce // 记录当前是否禁止ios回弹
      this.mescroll.setBounce(true) // 允许bounce
    }
    next()
  },
  activated(){
    if(this.timer){
      clearInterval(this.timer)
    }
    this.getKillList()
  },
  destroyed(){
    if(this.timer){
      clearInterval(this.timer)
    }
  },
  methods: {
    swiperChange (color) {
      this.swiperBg = color
    },
    openArticl(id){
      this.$router.push({name:'mallArticleInfo',query:{id:id}})
    },
    handDetail (id) {
      this.$router.push({
        name: 'mallHomeDetail',
        query: {
          title: id
        }
      })
    },
    openCart () {
      api.openWin({
        name: 'cartWin',
        url: '../html/cartmain.html',
        reload: true,
        animation: {
          type: 'push',
          subType: 'from_right',
          duration: 300
        },
        bounces: false,
        slidBackEnabled: false,
        allowEdit: true
      })
    },
    toList (item) {
      this.$router.push('/list?type=11&title=' + item.title + '&cid=' + this.cid)
    },
    rank (sort) {
      if (sort === 9) {
        this.multiple = !this.multiple
        
      } else if (sort === 0) {
        this.multiple = false
        this.sortName = '综合'
        if (this.classifySort === 0) {
        } else {
          this.classifySort = sort
          this.mescroll.resetUpScroll()
        }
      } else if (sort === 3) {
        this.multiple = false
        this.sortName = '优惠券面'
        if (this.classifySort === 3) {
        } else {
          this.classifySort = sort
          this.mescroll.resetUpScroll()
        }
      } else if (sort === 6) {
        this.sortName = '优惠券面'
        this.multiple = false
        if (this.classifySort === 6) {
        } else {
          this.classifySort = sort
          this.mescroll.resetUpScroll()
        }
      } else if (sort === 5) {
        if (this.classifySort === 5) {
          this.classifySort = 8
        } else {
          this.classifySort = 5
        }
        this.mescroll.resetUpScroll()
      } else if (sort === 2) {
        if (this.classifySort === 2) {
          this.classifySort = 1
        } else {
          this.classifySort = 2
        }
        this.mescroll.resetUpScroll()
      } else if (sort === 4) {
        if (this.classifySort === 4) {
          this.classifySort = 7
        } else {
          this.classifySort = 4
        }
        this.mescroll.resetUpScroll()
      }
    },
    ali () {
      api.openWin({
        name: 'aliWin',
        url: '../html/main.html',
        reload: true,
        pageParam: {
          url: 'https://s.click.taobao.com/g4LhrGw'
        },
        animation: {
          type: 'push',
          subType: 'from_right',
          duration: 300
        },
        bounces: false,
        slidBackEnabled: false,
        allowEdit: true
      })
    },
    navHttp (item) {
      if (item.url) {
        let browser = api.require('webBrowser')
        let url
        if (item.url.indexOf('http://') === 0 || item.url.indexOf('https://') === 0) {
          url = item.url
        } else {
          url = 'http://' + item.url
        }
        browser.open({
          url: url,
          titleBar: {
            textColor: '#333',
            bg: '#fff'
          }
        })
      }
    },
    refresh: function () {
      // this.categoryIndex = 0
      if(this.timer){
        clearInterval(this.timer)
        this.timer = ''
      }
      this.type = 0
      this.keyword = ''
      // this.cid = ''
      this.showMore = false
      this.scrollH = false
      this.mescrollUp.page.num = 1
      /*this.getDesign()
      this.getSwiper()
      */
      this.getKillList()
      this.getCategory()
      this.getMallDisplay()
      this.getMallGrid()
      this.upCallback(this.mescrollUp.page, this.mescroll)
    },
    getMallDisplay(){
      this.$http.post('/mall/index/display', {}, true, true).then((res) => {
        this.mallDisplay = res.data.navInfo;
        this.swiperslide = res.data.slide
        this.shopList = res.data.hotGoodsList
        this.article = res.data.article
      })
    },
    getMallGrid(){
      this.$http.post('/mall/index-nav/list', {}, true, true).then((res) => {
        this.shopGrid = res.data.list
      })
    },
    getKillList(){
       var that = this;
       this.$http.post('/mall/index-nav/kill-list', {}, true, true).then((res) => {
        this.killList = res.data;
        this.endTime = res.data.endTime;
        if(res.code==0){
          //秒杀接口调用成功,失败之后重新调用一下秒杀接口
          if(this.endTime>0){
          this.timer = setInterval(function(){
              that.transfer_time()
            },1000)
          }else{
            this.count = "00:00:00"
            this.kill = false
          }
        }else{
          this.getKillList()
        }
      })
    },
    transfer_time(){
        this.endTime = this.endTime -1
        
        let secondTime = this.endTime//this.data.endTime//parseInt(value); 秒
        let minuteTime = 0;// 分
        let hourTime = 0;// 小时
        let dayTime = 0; // 天
        let mouthTime = 0; // 月
        let yearTime = 0; //年
        if(secondTime > 60) {//如果秒数大于60，将秒数转换成整数
            //获取分钟，除以60取整数，得到整数分钟
            minuteTime = parseInt(secondTime / 60);
            //获取秒数，秒数取佘，得到整数秒数
            secondTime = parseInt(secondTime % 60);
            //如果分钟大于60，将分钟转换成小时
            if(minuteTime > 60) {
                //获取小时，获取分钟除以60，得到整数小时
                hourTime=parseInt(minuteTime/60);
                //获取小时后取佘的分
                minuteTime=parseInt(minuteTime%60);
                if(hourTime>24){
                    // 获取天数，获取小时除以24，得到整数天数
                    dayTime=parseInt(hourTime/24);
                    // 获取天数后取余的小时
                    hourTime=parseInt(hourTime%24);
                    // 一个月按30天算
                    if(dayTime>30){
                        // 获取月数，天数除以30，得到整月数
                        mouthTime=parseInt(dayTime/30);
                        // 获取月数取余后的天数
                        dayTime=parseInt(dayTime%30);
                        if(mouthTime>12){
                            // 获取年数，月数除以12，得到整年数
                            yearTime=parseInt(mouthTime/12);
                            // 获取年数取余后的月数
                            mouthTime=parseInt(mouthTime%12);
                        }
                    }
                }
            }
        }
        if(secondTime<10){
          var result = "0" + parseInt(secondTime);
        }else{
          if(secondTime==0){
            var result = "00" + parseInt(secondTime);
          }else{
            var result = "" + parseInt(secondTime);
          }
          
        }
        if(minuteTime > 0) {
            if(minuteTime<10){
              result = "0" + parseInt(minuteTime) + ":" + result;
            }else{
              result = "" + parseInt(minuteTime) + ":" + result;
            }
        }else{
          result = "0" + parseInt(minuteTime) + ":" + result;
        }
        if(hourTime >= 0) {
            if(hourTime<10){
              result = "0" + parseInt(hourTime) + ":" + result;
            }else{
              result = "" + parseInt(hourTime) + ":" + result;
            }
        }else{
          result = "00" + parseInt(hourTime) + ":" + result;
        }
        if(dayTime > 0) {
            result = "" + parseInt(dayTime) + "天" + result;
        }
        if(mouthTime > 0) {
            result = "" + parseInt(mouthTime) + "个月" + result;
        }
        if(yearTime > 0) {
            result = "" + parseInt(yearTime) + "年" + result;
        }
        this.count=result;
        
    },
    mescrollInit (mescroll) {
      this.mescroll = mescroll
    },
    upCallback (page, mescroll) {
      if (this.$store.state.global.content) {
        clearTimeout(this.times)
        if (!this.$store.state.global.goodsList || (this.$store.state.global.goodsList && this.$store.state.global.goodsList.link.type === 'tbk')) {
          this.tbk = true
          this.goodsList = false
          if (page.num !== 1) {
            page.num--
          }
          if (this.type === 1) {
            this.info = {
              page: page.num,
              keyword: this.keyword
            }
          } else if (this.type === 2) {
            this.info = {
              cid: this.cid
            }
          } else if (this.cid === '' || this.cid === 0) {
            this.info = {
              page: page.num,
              type: 1
            }
          } else {
            this.info = {
              page: page.num,
              cid: this.cid,
              sort: this.classifySort
            }
          }
          this.$http.post('/mall/index/goods-list', this.info, true, true).then((response) => {
            this.goodsList = true
            let arr = response.data.list
            if (page.num === 1) this.dataList = []
            // let p = page.num
            this.dataList = this.dataList.concat(arr)
            if (response.data && response.data.min_id) {
              this.mescrollUp.page.num = response.data.min_id
            } else {
              this.mescrollUp.page.num += 1
            }
            this.$nextTick(() => {
              mescroll.endSuccess(arr.length)
              
            })
          }).catch((e) => {
            mescroll.endErr()
          })
        } else {
          this.tbk = false
          this.goodsList = true
          let info
          if (this.$store.state.global.goodsList.link.type === 'hhtj') {
            info = {
              page: page.num,
              isRecommend: 1
            }
          } else if (this.$store.state.global.goodsList.link.type === 'spfl') {
            info = {
              page: page.num,
              cide: this.$store.state.global.goodsList.link.url
            }
          }
          this.$http.post('/mall/index/goods-list', info, true, true).then((response) => {
            let arr = response.data.list
            if (page.num === 1) this.dataList = []
            // let p = page.num
            this.dataList = this.dataList.concat(arr)
            this.$nextTick(() => {
              mescroll.endSuccess(arr.length)
            })
          }).catch((e) => {
            mescroll.endErr()
          })
        }
      } else {
        this.times = setTimeout(() => {
          this.upCallback(this.mescrollUp.page, this.mescroll)
        }, 1000)
      }
    },
    navTo (item) {
      if (item.type === 'BY_JD') {
        this.$router.push('/otherclassify?title=' + item.title + '&type=21')
      } else if (item.type === 'BY_PDD') {
        this.$router.push('/otherclassify?title=' + item.title + '&type=31')
      } else if (item.type === 'BY_DRS') {
        this.$router.push('/personSpeak')
      } else if (item.type === 'BY_MS') {
        this.$router.push('/fastrob')
      } else if (item.type === 'BY_MALL_INDEX') {
        this.$router.push('/mall/home')
      } else if (item.type === 'BY_MALL_OTO') {
        this.$router.push('/mall/underShop')
      } else if (item.type === 'BY_VIDEO') {
        this.$router.push('/videoList')
      } else if (item.type === 'BY_DRY') {
        this.$router.push('/dayingList')
      } else if (item.type === 'BY_ALI') {
        api.openWin({
          name: 'aliWin',
          url: '../html/main.html',
          reload: true,
          pageParam: {
            url: item.url
          },
          animation: {
            type: 'push',
            subType: 'from_right',
            duration: 300
          },
          bounces: false,
          slidBackEnabled: false,
          allowEdit: true
        })
      } else {
        this.$router.push('/classify?title=' + item.title + '&type=' + item.type)
      }
    },
    backTop () {
      setTimeout(() => {
        this.$refs.my_scroller.scrollTo(0, 0, true)
      }, 10)
    },
    getDesign () {
      this.$http.post('/amoy/app/design', {
        type: 1,
        version: api.appVersion,
        device: (api.systemType === 'android') ? 1 : 2
      }, true).then(res => {
        if (res.data.footer) {
          this.$store.commit('setFooter', JSON.parse(res.data.footer))
        }
        if (res.data.content) {
          this.$store.commit('setContent', JSON.parse(res.data.content))
        }
        if (res.data.theme) {
          this.$store.commit('setTheme', JSON.parse(res.data.theme))
        }
        if (res.data.is_app_store) {
          this.$store.commit('setFirstNav', '/mall/home')
          this.$router.push('/mall/home')
        } else {
          this.$store.commit('setFirstNav', '/home')
        }
        if (res.data.content) {
          let content = JSON.parse(res.data.content)
          for (let i = 0; i < content.length; i++) {
            if (content[i].name === 'edit-header') {
              this.$store.commit('setSearch', content[i].params)
            } else if (content[i].name === 'edit-goods-list') {
              this.$store.commit('setGoodsList', content[i].params)
            } else if (content[i].name === 'edit-xf-button') {
              this.$store.commit('setFloating', content[i].params)
              // alert(this.$store.state.global.floating)
              if (content[i].params && content[i].params.items) {
                for (let j in content[i].params.items) {
                  if (content[i].params.items[j].img) {
                    api.openFrame({
                      name: 'suspend_icon',
                      url: 'widget://html/suspend_icon.html',
                      rect: {
                        w: 70,
                        h: 70,
                        marginRight: 0,
                        marginBottom: 200
                      },
                      pageParam: {
                        img: content[i].params.items[j].img,
                        link: content[i].params.items[j].link
                      }
                    })
                  }
                }
              }
            }
          }
          let h = this.statusH + 0.88
          if (this.$store.state.global.content) {
            for (let i = 0; i < this.$store.state.global.content.length; i++) {
              if (this.$store.state.global.content[i].name === 'edit-header-category') {
                h = this.statusH + 0.88 + 0.78
              }
            }
          } else {
            h = this.statusH + 0.88 + 0.78
          }
          this.headBoxH = h
        }
      })
    },
    getSwiper () {
      this.$http.post('/amoy/home/slides', {}, true, true).then(res => {
        if (res.code === 0) {
          utils.storage.set('slideInfo', JSON.stringify(res.data))
        } else {
          utils.storage.set('slideInfo', JSON.stringify({}))
        }
      })
    },
    getCategory () {
      this.$http.post('/mall/index/category', {}, false).then(res => {
        this.category = res.data.list
      })
    },
    changeCate () {
      this.$http.post('/amoy/user/get-category', {cid: this.cid}).then(res => {
        this.other = res.data
      })
    },
    change (id, index) {
      this.mescroll.setScrollTop(0)
      this.type = 0
      this.categoryIndex = index
      this.showMore = true
      this.scrollH = true
      this.page = 1
      this.cid = id
      this.mescrollUp.page.num = 1
     // this.changeCate()
      this.getclassify(id)
     // this.mescroll.resetUpScroll()
    },
    getclassify(id){
       this.$http.post('/mall/index/category-list', {pid: id}).then(res => {
        if (res.code === 0) {
          this.secondList = res.data.list
        }
      })
    },
    scrollT () {
      this.$refs.my_scroller.scrollTo(0, this.$refs.recommend.offsetTop, false)
    }
  },
  created () {
    this.getSwiper()
    this.getCategory()
    let size = (document.documentElement.clientWidth / 7.5)
    
    this.statusH = (api.safeArea.top / size)
    let h = this.statusH + 0.88
    if (this.$store.state.global.content) {
      for (let i = 0; i < this.$store.state.global.content.length; i++) {
        if (this.$store.state.global.content[i].name === 'edit-header-category') {
          h = this.statusH + 0.88 + 0.78
        }
      }
    } else {
      h = this.statusH + 0.88 + 0.78
    }
    this.headBoxH = h
  },
  mounted: function () {
    
    this.getMallDisplay()
    this.getMallGrid()
    
   // document.getElementsByClassName('footer')[0] && (document.getElementsByClassName('footer')[0].style.paddingBottom = api.safeArea.bottom + 'px')
  }
}
</script>
<style lang="less">
  @import "../../assets/less/common";
  #home{
    .recommend {
      background: #fff;
      .goods_list_2, .goods_list_1 {
        .goods_ul {
          padding-top: 0;
        }
      }
    }
    .scrollable .vux-tab-item{
      flex: 0 0 14%;
    }
  }
  .home-box-hei{
    width:100%;
    height:0.2rem;
    background:#F7F7F7;
  }
</style>
<style scoped lang="less">
@import "../../assets/less/common";
.secondClassify{
  padding: .3rem 0 ;
  overflow: hidden;
  .classify{
    width: 20%;
    float: left;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    padding: .1rem 0;
    img{
      width: .8rem;
      height: .8rem;
      margin-bottom: .1rem;
    }
  }
}
.sortBox{
  position: sticky;
  top: -1px;
  background: #fff;
  z-index: 1000;
  display: flex;
  height: .9rem;
  border: 1px solid #F7F7F7;
  .sort{
    flex: 2;
    display: flex;
    align-items: center;
    justify-content: center;
    span{
      font-size: .28rem;
      color: #333;
      margin-right: .1rem;
    }
    img{
      width: .18rem;
    }
  }
  .changeStyle{
    flex: 1;
    img{
      width: .36rem;
    }
  }
  .multiple{
    position: absolute;
    bottom: -2.64rem;
    left: 0;
    width: 100%;
    background: #fff;
    div{
      font-size: .28rem;
      height: .88rem;
      line-height: .88rem;
      padding-left: .3rem;
    }
    .select{
      color: #FF2556;
    }
  }
}
.shadow{
  box-shadow:0 21px 36px -3px #EAEAEA;;
}
.eleven{
  /*width: 100%;*/
  height: 2rem;
  position: relative;
  margin: 0px 0.2rem;
  >img{
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
  }
  .go{
    position: absolute;
    top: 0.3rem;
    right: 0.1rem;
    width: 1rem;
    height: 1rem;
    animation:mymove 1s infinite;
    img{
      width: 100%;
      height: 100%;
    }
  }
}
@keyframes mymove
{
  from {
    transform: scale(1);
  }
  50% {
    transform: scale(1.2);
  }
  to {
    transform: scale(1);
  }
}
/*.swiper{*/
  /*height: 3.4rem;*/
/*}*/
.banner{
  height: 3.4rem;
  width: 100%;
  img{
    width: 100%;
    height: 100%;
  }
}
.explain{
  position: absolute;
  bottom: 0;
  width: 100%;
  font-size: 0.3rem;
  line-height: 0.9rem;
  padding-left: 0.2rem;
  color: black;
}
#home{
  background:#01C2C7;
  /*background: url("../../assets/img/home/homebg.png") no-repeat;*/
  /*background-size: contain;*/
  .headBox {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    z-index: 999;
  }
  .swiperBg{
    position: absolute;
    left: 0;
    top: 0;
    bottom: 0;
    width: 100%;
    z-index: 666;
    img{
      position: absolute;
      left: 0;
      right: 0;
      top: 4.5rem;
    }
    div{
      position: absolute;
      top: 6rem;
      left: 0;
      right: 0;
      bottom: 0;
      background: #fff;
      z-index: 667;
    }
  }
  .header{
    display: flex;
    justify-content: space-around;
    align-items: center;
    height: .88rem;
    padding: 0 .2rem;
    box-sizing: border-box;
    .searchBox{
      flex: 1;
      height: .6rem;
      border-radius: .3rem;
      background: rgba(255,255,255,.27);
      display: flex;
      align-items: center;
      justify-content: center;
      img{
        width: .3rem;
      }
      p{
        margin-top: 0.02rem;
        margin-left: .2rem;
        font-size: .26rem;
      }
    }
    .signIn{
      width: .40rem;
      height: .4rem;
      margin-right: .3rem;
    }
    .shopImg{
      width: .40rem;
      height: .4rem;
      margin-left: .3rem;
    }
    .shop{
      font-size: .6rem;
      line-height: .6rem;
      height: 100%;
      width: .6rem;
      position: relative;
      top: .075rem;
      left: .075rem;
      img{
        width: .45rem;
        height: .45rem;
      }
      .vux-badge{
        background: #fff;
        position: absolute;
        right: 0.01rem;
        top: 0.01rem;
      }
    }
    .iconfont{
      color: #fff;
      font-size: .4rem;
    }
  }
  .dialog{
    position: fixed;
    left: 0;
    top: 200px;
    bottom: 0;
    right: 0;
    background: rgba(0,0,0,.4);
    z-index: 866;
  }
  .moreBox{
    z-index: 999;
    position: absolute;
    top: 0.78rem;
    left: 0;
    width: 100%;
    box-sizing: border-box;
    height:auto;
    min-height:5rem;
    background: white;
    padding: .2rem .2rem;
    padding-top:0.5rem;
    .t{
      overflow: hidden;
      span{
        font-size: .26rem;
        float: left;
      }
      i{
        float: right;
      }
    }
    .classify{
      .classifyBox{
        width: 15%;
        margin: .2rem 5%;
        float: left;
        display: flex;
        justify-content: center;
        flex-direction: column;
        align-items: center;
        img{
          width: .9rem;
          height:0.9rem;
          margin-bottom: .2rem;
        }
        span{
          font-size: .26rem;
        }
      }
    }
  }
  ._v-container{
    top: 44px;
  }
  height: 100%;
  position: relative;
  .showlist-enter-active,
  .showlist-leave-active {
    transition: all .3s;
  }
  .showlist-enter,
  .showlist-leave-active {
    opacity: 0;
  }
  .navBox{
    height: 0.78rem;
    position: relative;
    z-index: 100;
  }
  .tabBox{
    display: flex;
    align-items: center;
    height: 0.78rem;
    position: relative;
    z-index: 100;
    .tabIndex{
      flex: 1;
      position: relative;
      z-index: 1000;
      overflow-y: auto;
      display: -webkit-box;
      flex-wrap: nowrap;
      height: 100%;
      color:white;
      div{
        padding: 0 .2rem;
        height: 100%;
        font-size: .26rem;
        display: flex;
        align-items: center;
        justify-content: center;
      }
      .active{
        font-size: .26rem;
      }
    }
    .vux-tab-wrap{
      flex: 1;
      z-index: 100;
    }
    .vux-tab-item{
      font-size: .26rem;
    }
    .vux-tab-selected{
      font-weight: bold;
      font-size: .28rem;
    }
     .vux-tab-item{
      /*color: #fff !important;*/
    }
    .more{
      position: relative;
      z-index: 100;
      height: 44px;
      display: flex;
      align-items: center;
      padding: 0 .2rem;
      .icon-sort{
        font-size: .4rem;
        color:white;
      }
    }
    .more:before{
      content: " ";
      position: absolute;
      left: 0;
      bottom: 9px;
      top: 9px;
      height: 26px;
      border-left: 1px solid #fff;
      color: #fff;
      -webkit-transform-origin: 0 100%;
      transform-origin: 0 100%;
      -webkit-transform: scaleX(0.5);
      transform: scaleX(0.5);
    }
  }
  .list{
    width: 100%;
    position: absolute;
    bottom: 0;
    overflow-y: auto;
    z-index: 777;
    .high{
      background: @h-col;
      height: .1rem;
      width: 100%;
    }
  }
  .saleBox{
    overflow: hidden;
    background: #fff;
    .saleT{
      position: relative;
      width: 7rem;
      height: 2.1rem;
      padding: .2rem .25rem;
      img{
        height: 100%;
        width: 100%;
      }
      p{
        position: absolute;
        left: 0;
        top: .41rem;
        width: 100%;
        text-align: center;
        font-size: .32rem;
        line-height: .5rem;
        color: #fff;
      }
    }
    .imgNav{
      overflow: hidden;
      padding: .2rem .3rem;
      img{
        float: left;
        width: 3.44rem;
        height: 2.47rem;
        box-sizing: border-box;
      }
      img:nth-of-type(1){
        border-right: #f3f3f3 solid 1px;
      }
      img:nth-of-type(2){
      }
      img:nth-of-type(3){
        border-top: 1px solid #f3f3f3;
        border-right: 1px solid #f3f3f3;
      }
      img:nth-of-type(4){
        border-top: 1px solid #f3f3f3;
      }
    }
    .sales{
      padding-bottom: .2rem;
      display: flex;
      img{
        width: 100%;
        height: 100%;
      }
      .sale{
        margin-left: .18666rem;
        width: 3.47rem;
        height: 4.3rem;
        .sale1{
          width: 100%;
          height: 2.11rem;
          margin-bottom: .08rem;
        }
        .sale2{
          width: 100%;
          height: 2.11rem;
        }
      }
      .sale3{
        margin-left: .18666rem;
        width: 3.47rem;
        height: 4.3rem;
      }
    }
  }
  .saleBoxs{
    height: 7rem;
    position: relative;
    padding: 0.16rem;
    > img{
      width: 100%;
      height: 100%;
    }
    .imgBox{
      position: absolute;
      width: 100%;
      height: 0.5rem;
      top: 2.4rem;
      height: 4.56rem;
      overflow: hidden;
      .le{
        float: left;
        width: 60%;
        height: 100%;
        display: flex;
        flex-direction: column ;
        .bottoms{
          width: 90%;
          margin: 0 auto;
          height: 50%;
          margin-top: 0.1rem;
        }
        .tops{
          height: 46%;
          width: 90%;
          margin: 0 auto;
        }
        img{
          width: 100%;
          height: 100%;
        }
      }
      .ri{
        width: 34%;
        height: 98%;
        float: right;
        position: absolute;
        right: 0.55rem;
        img{
          width: 100%;
          height: 100%;
        }
      }
    }
  }
  .commodityBox{
    padding: 0.16rem;
    margin-top: 0.7rem;
    .commodity{
      display: flex;
      height: 2.72rem;
      border-bottom: 1px solid #e1e1e1;
      div{
        flex: 1;
        padding: 0.2rem;
        img{
          width: 100%;
        }
      }
      >div:nth-child(1){
        border-right: 1px solid #e1e1e1;
      }
    }
    .active{
      border-bottom: none
    }
  }
  .recommendT{
    .t{
      width: 100%;
      height: 1.2rem;
      margin: 0 auto;
      display: flex;
      align-items: center;
      justify-content: center;
      img{
        width: 5.68rem;
      }
    }
  }
  .recommend{
    background: #fff;
    .goods_list_2, .goods_list_1{
      .goods_ul{
        padding-top: 0;
      }
      height: calc(100% - 1.2rem);
      background: #f4f4f4;
      ul{
        height: 100%;
        overflow: auto;
      }
    }
  }
  .over{
    overflow-y: hidden;
  }
}
/*16格*/
.listBox {
      /*.vux-tab .vux-tab-item.vux-tab-selected {*/
        /*color: #FF7265;*/
        /*!*border-bottom: 3px solid #FF7265;*!*/
        /*border-bottom: none !important;*/
      /*}*/
      
      background-color: #fff;
      .listTitle {
        width: 2.95rem;
        margin: 0 auto;
        padding: .5rem 0;
        display: flex;
        justify-content: space-between;
        font-size: .30rem;
        line-height: .30rem;
        img {
          width: .48rem;
          height: .30rem;
          vertical-align: middle;
        }
      }
      .listItem {
        display: flex;
        flex-wrap: wrap;
        align-items: start;
        li {
          display: flex;
          flex-direction: column;
          align-items: center;
          justify-content: space-between;
          width: 25%;
          box-sizing: border-box;
          
         /* &:nth-of-type(4n+0) {
            border-right: none;
            //padding-bottom: .25rem;
          }
          &:nth-child(n+5) {
            border-bottom: none;
          }*/
          .logoBox {
            width:1.5rem;
            height:1.5rem;
            
            img {
              width: 100%;
              vertical-align: middle;
            }
          }
          p {
            font-size: .26rem;
            width: 100%;
            overflow: hidden;
            text-overflow:ellipsis;
            white-space: nowrap;
            text-align: center;
          }
          .average {
            font-size: .2rem;
            color: #999;
          }
        }
      }
      .tabBar {
        box-sizing: border-box;
        margin-bottom: .5rem;
        .vux-tab-container {
          .scrollable {
            padding-bottom: 7px;
          }
        }
        .vux-tab-ink-bar{
          bottom: 28px !important;
        }
      }
    }
    .homeactive{
      color:white !important;
      font-size:0.3rem;
      font-weight:bold;
      border-bottom:2px solid white;
    }
    .kill-tit{
      width:100%;
      display:flex;
      justify-content: space-between;
      align-items:center;
      box-sizing: border-box;
      height:1rem;
      padding:0.3rem;
      background:white;
      div:first-child{
        font-weight:bold;
        color:#333333;
        font-size:0.3rem;
      }
      div:last-child{
        width: 1.2rem;
        height: 0.4rem;
        line-height: 0.4rem;
        text-align: center;
        font-size: 0.25rem;
        color: #FF1D1C;
        border-radius: 5px;
        border: 1px solid #FF1D1C;
      }
    }
  .mall-poster{
    width:100%;
    max-height:3rem;
    text-align: center;
    background:white;
  }
  .home-active{
    color:orangered
  }
</style>
