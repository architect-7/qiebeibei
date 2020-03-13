<template>
    <div class="GoodsList" >
      <div :class="{goods_list_2 : !styles, goods_list_1 : styles}" v-if="list[0]">
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
              <div  style="font-size:0.25rem;display:flex;justify-content:space-between;align-items:center;">
                <p style="color:#999999"><span style="margin-left:5px;" v-for="(it,ind) in item.goodsTag" :key="ind">{{it}}</span></p> <p  style="color:#FF1D1C;background:#FFE9E9;border-radius:0.1rem;"  class="g-red">仅剩: {{item.total}}{{item.unit}}</p> 
              </div>
              <div class="group-now">
                  <div class="g-left">单价：￥<span class="s-1">{{item.spe_price}}</span> </div>
                  <div class="g-right" @click="openGroup(item)"><span class="s-1">抢购</span></div>
              </div>
            </div>
          </li>
        </ul>
        <div id="empty"></div>
      </div>
      <div class="no-data" v-if="!list[0]"> 暂无数据</div>
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
        this.$router.push({name: 'mallUnderShopDetail', query: {id: item.id,type:'protect'}})
    }
  },
  mounted () {
    
  }
}
</script>

<style scoped lang="less">
.no-data{
  width:100vw;
  height:2rem;
  line-height:2rem;
  text-align: center;
}
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
        width:1.4rem;
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
