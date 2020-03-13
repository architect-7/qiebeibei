<template>
  <div class="hhhe">
    <mescroll-vue ref="mescroll"  :down="mescrollDown" :up="mescrollUp" @init="mescrollInit">
    <div class="nav">
     <div class="money-header"><img @click="goBack()" src="../../assets/img/oli/left.png" alt=""><span>我的钱包</span></div>
     <!-- <img src="../../assets/img/oli/qianb.png" alt="">
      <p>我的余额</p>
      <p>￥{{moneymsg.price ? moneymsg.price :0 }}</p>-->
      <div class="money-box">
        <div class="row-1">我的余额</div>
        <div class="row-2">{{moneymsg.price ? moneymsg.price :0 }}</div>
        <div class="row-3">
          <div @click="jumpTo('/mall/yuePayment')">充值</div> 
          <div @click="jumpTo('/mall/yuWithdraw')">提现</div>
        </div>
      </div>
    </div>
    <!-- 记录 -->
    <div class="buyway">
      <span>收支明细</span>
    </div>
    <!-- 具体记录 -->
    <div class="forpp">
      <ul class="cleann">
        <li v-for="(item, index) in alllist" :key="index" > 
            <div>
              <div class="yingzizi">
                <p>{{types[item.types]  ? types[item.types]
                     :item.types}}</p>
                <p>{{item.created_at ? item.created_at : null }}</p>
              </div>
            </div>
            <div class="getgo">{{item.price ? item.price : null}}</div>
        </li>
      </ul>
    </div>
    </mescroll-vue>
  </div>
</template>

<script>
import MescrollVue from 'mescroll.js/mescroll.vue'
export default {
  components:{
    MescrollVue
  },
  data() {
    return {
      list: {},
      alllist: [],
      types:[],
      page: 0,
      monn:this.$store.state.user.userInfo.credit2,
      mescroll: null, //  添加上拉数据
      mescrollDown: {},
      moneymsg:'',
      mescrollUp: {
        callback: this.upCallback,
        page: {
          num: 0,
          size: 5
        },
        htmlNodata: '<p class="upwarp-nodata">-- END --</p>',
        htmlLoading: '',
        noMoreSize: 5,
        toTop: {
          src: './static/img/auth/back_top.png', // 图片
          offset: 1000
        },
        empty: {
          warpId: 'empty',
          icon: './static/img/auth/kong.png',
          tip: '暂无相关数据~'
        }
      },
      lastScrollTop: 0, // 路由切换时滚动条的位置
      lastBounce: null // 路由切换时是否禁止ios回弹
    };
  },
  created() {
    this.getmsg()
  },
  methods: {
    // 下拉加载的操作
    mescrollInit (mescroll) {
      this.mescroll = mescroll
    },
    getmsg(){
      this.$http.post('/mall/credit/credit2-info', {}, true, true).then((res) => {
        this.moneymsg = res.data
      })
    },
    upCallback (page, mescroll) {
      this.mescrollUp.htmlLoading = '<p class="upwarp-progress mescroll-rotate"></p><p class="upwarp-tip">加载中..</p>'
      this.$http.post('/mall/credit/credit2', {
        page: page.num
      }, true, true).then((res) => {
        if (res.code === 0) {
          this.types=res.data.typesInfo
          
          let arr = res.data.list
         
          if (page.num == 1) this.alllist = []
          this.alllist = this.alllist.concat(arr)
          
          this.$nextTick(() => {
            mescroll.endSuccess(arr.length)
          })
        } else {
          mescroll.endErr()
        }
      }).catch((e) => {
        mescroll.endErr()
      })
    },
    reset (status) {
      this.mescrollUp.htmlLoading = ''
      this.sort = status
      this.mescrollUp.page.num = 1
      this.finish = false
      this.upCallback(this.mescrollUp.page, this.mescroll)
    }   // 下拉加载的操作
  }
};
</script>

<style scoped>
.mescroll {
   position: fixed;
   top: 2px;
   bottom: 0;
   height: auto;
}
.activ {
  position: absolute;
  right: 0.3rem;
  bottom: 0.01rem;
  font-size: 0.31rem;
  color: rgba(102, 102, 102, 1);
}
/* 背景 */
.nav {
  width: 100%;
  height: 3.56rem;
  background: url("../../assets/img/oli/qianbaobj.png") no-repeat center center;
  background-size: cover;
  font-size: 0.3rem;
  text-align: center;
  box-sizing: border-box;
  
}
/* .nav img {
  height: 1rem;
}
提现按钮， 和转账
.nav span {
   display: inline-block;
   width: 1.58rem;
   height: .52rem;
   line-height: .52rem;
   border: .5px solid #fff;
   border-radius:10px; 
   font-size: .28rem;
   color: #fff;
   margin-top: .12rem;
   font-weight:400;
}
.nav span:nth-of-type(1) {
  margin-right:1.3rem; 
}
.nav span:nth-of-type(2) {
  margin-left:.3rem; 
}
.nav p {
  line-height: 0.52rem;
}
.nav p:nth-of-type(1) {
  font-size: 0.3rem;
  font-weight: 500;
  color: rgba(255, 255, 255, 1);
}
.nav p:nth-of-type(2) {
  font-size: 0.34rem;
  font-weight: 700;
  color: rgba(255, 255, 255, 1);
} */
/* 收支明细入 */
.buyway {
  position: relative;
  width: 100%;
  height: 0.92rem;
  line-height: 0.92rem;
  box-sizing: border-box;
  background-color: #fff;
  border-bottom: 1px solid rgba(237, 237, 237, 1);
  margin-top:1rem;
}
.buyway span {
  position: absolute;
  top: 0;
  left: 0.85rem;
  font-size: 0.33rem;
  font-weight: 500;
  color: rgba(51, 51, 51, 1);
}
.buyway::after {
  content: "";
  position: absolute;
  top: 0.23rem;
  left: 0.3rem;
  width: 0.4rem;
  height: 0.4rem;
  background: url("../../assets/img/oli/mign.png") no-repeat center center;
  background-size: 100% 100%;
  border-radius: 2px;
}
/* li的设置哦 */
.forpp {
  position: relative;
  width: 100%;
  font-size: 0.4rem;
  background-color: #fff;
}
.forpp ul {
  padding: 0 0.3rem;
}
.forpp li {
  position: relative;
  box-sizing: border-box;
  padding: 0.25rem 0rem 0 0.1rem;
  height: 1.4rem;
  border-bottom: 1px solid rgba(237, 237, 237, 1);
  list-style: none;
}
.forpp li div:nth-of-type(1) {
  position: relative;
}
.yingzizi {
  position: absolute;
  top: -0rem;
}
/* 右边的字 */
.getgo {
  position: absolute;
  right: 0.25rem;
  bottom: 0.4rem;
  font-weight: 500;
  color: #01C2C7;
  font-size: 0.32rem;
}
/* 左侧的两排子 */
.yingzizi p:nth-of-type(1) {
  font-weight: 500;
  color: rgba(51, 51, 51, 1);
  font-size: 0.3rem;
}
.yingzizi p:nth-of-type(2) {
  font-weight: 400;
  color: rgba(153, 153, 153, 1);
  font-size: 0.26rem;
  line-height: 0.6rem;
}
/* 没有更多 */
.nono {
  font-size: 0.3rem;
  margin: 0rem auto;
  text-align: center;
  font-weight: 400;
  color: rgba(153, 153, 153, 1);
  line-height: 40px;
}
.money-box{
  width:7rem;
  height:3rem;
  background:white;
  border-radius:0.2rem;
  margin:0.5rem auto;
  text-align:left;
  box-sizing: border-box;
  padding:0.3rem;
}
.money-box>.row-1{
  color:#666666
}
.money-box>div{
  margin-bottom:0.25rem;
}
.money-box>.row-2{
  font-size:0.6rem;
  font-weight:bold;
  color:#FF3635;
}
.money-box>.row-3{
  width:100%;
  display:flex;
  justify-content: space-around;
  align-items: center;
}
.money-box>.row-3>div{
  display:inline-block;
  width:1.8rem;
  height:0.6rem;
  line-height: 0.6rem;
  border-radius:0.1rem;
  text-align: center;
}
.money-box>.row-3>div:first-child{
  border:1px solid #01C2C7;
  color:#01C2C7;
}
.money-box>.row-3>div:last-child{
  background: #01C2C7;
  color:white;
}
.money-header{
  width:100%;
  height:1rem;
  padding:0.3rem;
  font-size:0.4rem;
  box-sizing: border-box;
}
.money-header>img{
  width:0.3rem;
  height:0.3rem;
}
.money-header>span{
  display:inline-block;
  width:6rem;
  text-align: center;
  color:white;
  font-weight:bold
}
</style>