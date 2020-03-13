<template>
    <div class="GoodsList" >
      <div :class="{goods_list_2 : !styles, goods_list_1 : styles}">
        <ul class="goods_ul">
          <li class="goods_li" v-for="(item,index) of list" :key="index" @click="openGroup(item)">
            <div class="preview">
                <img v-lazy="item.thumb" alt="" :key="item.thumb">
            </div>
            <div class="con_right">
                <p class="brand" style="font-size:0.3rem;">
                    {{item.goodsName}}
                </p>
                <div  style="font-size:0.25rem;display:flex;justify-content:space-between;align-items:center;">
                    <p style="color:#999999">
                        <span style="color:#FF1D1C;margin-right:.2rem;">￥{{item.spe_price}}</span>
                        <del>￥{{item.marketprice}}</del>
                    </p> 
                </div>
            </div>
          </li>
        </ul>
        <div id="empty"></div>
      </div>
    </div>
</template>

<script>
import { Marquee, MarqueeItem } from 'vux'
import * as utils from '../utils'
export default {
  name: 'goldenKey',
  components: { Marquee,
    MarqueeItem},
  props: ['list', 'styles', 'showVideo'],
  data () {
    return {}
  },
  methods: {
    openGroup(item){
      this.$http.post('/amoy/user/user-info', {}).then(res => {
        if(res.code ==0 && res.data.authentication=='1'){
          utils.storage.set('keykeyDetail', item)
          this.jumpTo('/goldenKeykeyDetail')
        }else{
           this.$vux.toast.text('请实名认证！')
           setTimeout(()=>{
             this.jumpTo('/authentication')
           },1000)
        }
      })
    }
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
