 <template>
  <div  class="myCoupon" >
    <x-header
        slot="header"
        :left-options="{showBack: false}"
      >
        <a @click="goBack()" slot="left" class="backBox"><i class="iconfont icon-back"></i></a>
        优惠券
        <p style="position: absolute;right: .3rem;top: 0;color: rgba(153,153,153,1);font-size:.28rem;" @click="jumpTo('/coupon_rule')">使用规则</p>
    </x-header>
    <div style="height:1rem"></div>
    <div class="coupon-tab">
        <div @click="switchTab(0)" :class="{'coupon-active':tab==0}">可使用</div>
        <div @click="switchTab(1)" :class="{'coupon-active':tab==1}">已使用</div>
        <!--<div @click="switchTab(3)" :class="{'coupon-active':tab==3}">已失效</div>-->
    </div>
    <div class="coupon-item" v-for="(item,index) of couponList" :key="index" @click="getCoupon(item)">
          <div class="coupon-left" :class="{'do-use':tab==1}">
              <div>￥ <span class="coupon-1">{{item.sub}}</span></div>
              <div :class="{'do-use':tab==1}">{{item.couponType}}</div>
          </div>
          <div class="coupon-right" >
              <div class="coupon-r-left">
                  <div class="coupon-right-1">{{item.types}}</div>
                  <div class="coupon-right-full">满{{item.full}}可使用</div>
                  <div style="color:gray">发券时间：{{item.created_at}}</div>
              </div>
              <div v-show="tab==0" class="coupon-r-right" @click="goUse(item)">去使用</div>
              <div v-show="tab==1" class="coupon-r-right" :class="{'do-use':tab==1}" >已使用</div>
          </div>
      </div>
      <div class="no-data">暂无数据</div>
  </div>
</template>

<script>
import * as utils from '../../utils'
export default {
  name: 'myCoupon',
  components: {
    
  },
  data () {
    return {
      tab:0,
      couponList:[]
    }
  },
  methods: {
    switchTab(type){
        this.tab = type
        this.getCoupon()
    },
    goUse(item){
        if(item.goodsId==0||item.goodsId=='0'){
            this.$router.push('/home')
        }else{
            this.$router.push({name:'mallHomeDetail',query:{title:item.goodsId}})
        }
    },
    getCoupon(){
      this.$http.post('mall/coupon/user-coupon-list', {status:this.tab}, false).then(res => {
        this.couponList = res.data.list;
      })
    }
  },
  mounted () {
    this.getCoupon()
  }
}
</script>
<style scoped lang="less">
  .myCoupon{
      .coupon-tab{
          width:100%;
          height:1rem;
          background:white;
          display:flex;
          justify-content: space-around;
          align-items: center;
          font-size:0.3rem;
          div{
              width:1rem;
              text-align: center
          }
      }
      .no-data{
        width:100vw;
        height:2rem;
        text-align: center;
        line-height: 2rem;
      }
      .coupon-item{
        width:7rem;
        height:1.5rem;
        position:relative;
        left:0.3rem;
        margin-bottom:0.3rem;
        border-radius:0.2rem;
        margin-top:0.2rem;
        display:flex;
        font-size:0.25rem;
        .coupon-left{
            width:1.6rem;
            height:1.5rem;
            border-radius:0.2rem;
            color:white;
            text-align:center;
            background: -webkit-gradient(linear,center top,center bottom,from(#3FE7E5), to(#10A9B0));
            .coupon-1{
                font-size:0.5rem;
                font-weight:bold;
            }
            div:last-child{
                width:1rem;
                height:0.4rem;
                text-align: center;
                line-height: 0.4rem;
                border-radius:0.2rem;
                background:#10A9B0;
                color:white;
                margin:0.2rem auto;
            }
        }
        .coupon-right{
            width:5.4rem;
            height:1.5rem;
            background:white;
            border-radius:0.2rem;
            box-sizing: border-box;
            padding:0.2rem;
            display:flex;
            justify-content: space-between;
            align-items: center;
            .coupon-r-right{
                width:1rem;
                height:0.5rem;
                line-height: 0.5rem;
                text-align: center;
                background:#01C2C7;
                color:white;
                border-radius:0.1rem;
                img{
                    max-width:1.1rem;
                    max-height:0.7rem;
                }
            }
            .coupon-right-1{
                font-weight:bold;
                color:#01C2C7
            }
            .coupon-right-full{
                margin-top:0.10rem;
                margin-bottom:0.10rem;
            }
        }
    }
  }
  .coupon-active{
      font-weight:bold;
      color:#333333;
      border-bottom:2px solid #01C2C7;
  }
  .do-use{
    background:rgb(235,235,235) !important;
  }
</style>
