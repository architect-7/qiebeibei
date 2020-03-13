<template>
  <div class="hhhe">
     <!-- 滚动容器 用 mescroll 包括 -->
    <mescroll-vue ref="mescroll"  :down="mescrollDown" :up="mescrollUp" @init="mescrollInit">
      <div class="nav">
        <img src="../../assets/img/oli/shu.png" alt="">
        <p>数字资产</p>
        <p>￥{{meo}}</p>
      </div>
      <!-- 记录 -->
      <div class="buyway">
        <span>收支明细</span>
      </div>
      <!-- 具体记录 -->
      <div class="forpp">
        <ul class="cleann">
          <li v-for="(item, index) in alllist" :key="index" v-show="item.types == 'oil6' || item.types == 'speed_oil6'"> 
            <div>
              <div class="yingzizi">
                <p>{{item.types=="oil6" ? "数字资产" : item.types=="speed_oil6" ? "加速获取数字资产" :null}}</p>
                <p>{{item.created_at}}</p>
              </div>
            </div>
            <div class="getgo">{{'+'+ item.price}}</div>
          </li>
        </ul>
      </div>
    </mescroll-vue>
  </div>
</template>

<script> 
import MescrollVue from 'mescroll.js/mescroll.vue' // 下载 ， 引入
export default {
  components:{
    MescrollVue  // 挂载 
  },
  computed:{
    
  },
  data() {
    return {
      list: {},
      alllist: [],
      page: 1,
      meo:this.$store.state.user.userInfo.credit6,
      mescroll: null, //  添加上拉数据
      mescrollDown: {}, // 下拉刷新的配置 ，如 跟上拉一致，就不要写了；
      mescrollUp: {   // 上拉刷新 配置
        callback: this.upCallback, //  上拉的回调
        page: {
          num: 0, //当前页 默认0,回调之前会加1; 即callback(page)会从1开始
          size: 5 //每页数据条数,默认
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
    // alert(JSON.stringify(this.$store.state.user.userInfo.credit6))
  },
  methods: {
    mescrollInit (mescroll) { // 下拉加载的操作
      this.mescroll = mescroll
    },
    upCallback (page, mescroll) { // 上拉回调
      this.mescrollUp.htmlLoading = '<p class="upwarp-progress mescroll-rotate"></p><p class="upwarp-tip">加载中..</p>'
      this.$http.post('/amoy/user/get-oil-type', {
        page: page.num,
        type:"credit6"
      }, true, true).then((res) => {
        if (res.code === 0) {
          let arr = res.data
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
  padding-top: 1.4rem;
  height: 4.56rem;
  background: url("../../assets/img/oli/bgc.png") no-repeat center center;
  background-size:cover;
  font-size: 0.3rem;
  text-align: center;
  box-sizing: border-box;
  margin-top: 1.35rem;
}
.nav img {
  height: 1rem;
}
/* 提现按钮， 和转账 */
.nav span {
   display: inline-block;
   width: 1.58rem;
   height: .52rem;
   line-height: .52rem;
   border: .5px solid #fff;
   border-radius:10px; 
   font-size: .28rem;
   color: #fff;
   margin-top: .1rem;
   font-weight:400;
}
.nav span:nth-of-type(1) {
  margin-right:1.3rem; 
}
.nav span:nth-of-type(2) {
  margin-left:.3rem; 
}
.nav p {
  line-height: 0.6rem;
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
}
/* 收支明细入 */
.buyway {
  position: relative;
  width: 100%;
  height: 0.92rem;
  line-height: 0.92rem;
  box-sizing: border-box;
  background-color: #fff;
  border-bottom: 1px solid rgba(237, 237, 237, 1);
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
  color: rgba(255, 62, 85, 1);
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
</style>