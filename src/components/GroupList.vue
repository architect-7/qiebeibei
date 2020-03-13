<template>
    <div class="GoodsList" >
      <div :class="{goods_list_2 : !styles, goods_list_1 : styles}">
        <ul class="goods_ul">
          <li class="goods_li" v-for="item of list" :key="item.origin_id" @click="detail(item)">
            <div class="preview">
              <img v-lazy="item.thumb" alt="" :key="item.thumb">
              <img src="../assets/img/home/video.png" alt="" class="video" v-if="item.video_url && showVideo"  @click.stop="openVideo(item)">
              <p class="commission_money" v-if="item.commission_money">分享赚：{{item.commission_money}}</p>
            </div>
            <div class="con_right">
              <p class="brand" style="font-size:0.3rem;">
                {{item.goodsName}}
              </p>
              <!--<p class="listinfo">{{item.description}}</p>-->
              <div  style="font-size:0.25rem;display:flex;justify-content:space-between;align-items:center;">
                <p style="color:#999999"><del>单价买:&nbsp;&nbsp;￥{{item.spe_price}}</del></p> <p  style="color:#FF1D1C;background:#FFE9E9;border-radius:0.1rem;"  class="g-red">仅剩: {{item.total}}</p> 
              </div>
              <div class="group-now">
                  <div class="g-left">￥ <span class="s-1">{{item.group_buy_price}}</span> </div>
                  <div class="g-right" @click="openGroup(item)"><span>两人团</span><span class="s-1">去开团</span></div>
              </div>
              <!--<div class="price"><span class="later" :style="{background: 'linear-gradient('+$store.state.global.theme.deg+'deg,' +$store.state.global.theme.mainColor+','+$store.state.global.theme.secondColor+')'}">券后价</span><p :style="{color: $store.state.global.theme.mainColor}">￥<span>{{item.coupon_price}}</span></p><b :style="{background: 'linear-gradient('+$store.state.global.theme.deg+'deg,' +$store.state.global.theme.mainColor+','+$store.state.global.theme.secondColor+')'}">{{item.coupon_money || 0}}元券</b></div>
              <div class="up_gain" v-if="$bizId !== 1 && item.level_commission_money && item.level_commission_money != 0">
                <span v-if="item.commission_money">预估赚:￥{{item.commission_money}}</span>
                <span v-if="item.level_commission_money && item.level_commission_money != 0 && (item.commission_money != item.level_commission_money)">升级赚:￥{{item.level_commission_money}}</span>
              </div>-->
            </div>
          </li>
        </ul>
        <div id="empty"></div>
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
    detail (item) {
      this.$emit('detail', item)
    },
    openVideo (item) {
      this.$emit('openVideo', item)
    },
    openGroup(item){
        this.$router.push({
        name: 'gdetails',
        query: {
          id: item.id
        }
      })
    }
  },
  mounted () {
  }
}
</script>

<style scoped lang="less">
.group-now{
    width:100%;
    font-size:0.3rem;
    color:black;
    display:flex;
    justify-content: space-between;
    align-items:center;
    .g-left{
        font-weight:bold;
        color:#FF1D1C;
        .s-1{
            font-size:0.35rem;
        }
    }
    .g-right{
        width:2.4rem;
        height:0.5rem;
        border-radius:0.1rem;
        border:1px solid #01C2C7;
        span{
            display:inline-block;
            width:1.2rem;
            height:0.5rem;
            border-top-right-radius: 0.1rem;
            border-bottom-right-radius: 0.1rem;
            text-align: center;
            line-height: 0.5rem;
        }
        .s-1{
            background:#01C2C7;
            color:white;
        }
    }
}



</style>
