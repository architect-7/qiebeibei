<template>
    <div class="GoodsList" >
      <div class="coupon-item" v-for="(item,index) of list" :key="index" @click="getCoupon(item)">
          <div class="coupon-left" v-if="item.goods_id!=='0'">
              <div>￥ <span class="coupon-1">{{item.sub}}</span></div>
              <div>领取</div>
          </div>
          <div class="coupon-left" v-if="item.goods_id=='0'">
              <div>￥ <span class="coupon-1">{{item.sub}}</span></div>
              <div>{{item.coupon_type}}</div>
          </div>
          <div class="coupon-right" v-if="item.goods_id!=='0'">
              <div class="coupon-r-left">
                  <div class="coupon-right-1">{{item.goodsName}}商品使用</div>
                  <div class="coupon-right-full">满{{item.full}}可使用</div>
                  <div style="color:gray">发券时间：{{item.created_at}}</div>
              </div>
              <div class="coupon-r-right"><img :src="item.thumb" alt=""></div>
          </div>
          <div class="coupon-right" v-if="item.goods_id=='0'">
              <div class="coupon-r-left">
                  <div class="coupon-right-1">{{item.type}}</div>
                  <div class="coupon-right-full">满{{item.full}}可使用</div>
                  <div style="color:gray">发券时间：{{item.created_at}}</div>
              </div>
              <div class="coupon-r-right">领取</div>
          </div>
      </div>
    </div>
</template>

<script>
export default {
  name: 'GroupList',
  components: {},
  props: ['list', 'styles', 'showVideo'],
  data () {
    return {}
  },
  methods: {
    getCoupon(item){
      this.$http.post('mall/coupon/get-coupon', {id:item.id}, false).then(res => {
        if(res.code==0){
            this.$vux.toast.text(res.msg);
        }else{
            this.$vux.toast.text(res.msg);
        }
      })
    }
  },
  mounted () {
  }
}
</script>

<style scoped lang="less">
.GoodsList{
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
</style>
