<template>
  <div class="home_list" v-if="list">
    <div class="header_count">
      <ul>
        <li v-for="(item,index) in list"  @click="handDetail(item.id,item.deduce)" :key="index">
          <img v-lazy="item.thumb" alt="">
          <div class="shop_name">
            <table></table>
            <p>{{item.goodsName}}</p>
            <!--<div class="market-price">
              <del>¥ <span>{{item.marketprice}}</span></del>
            </div>-->
            <div class="price">
              <div class="price_main"><span class="ticket">{{parseInt(item.integral)}}积分+{{item.spe_price}}元</span></div>
              <span style="font-size:3vw">已兑换：{{item.sales}}</span>
            </div>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'tlist',
  props: ['list'],
  methods: {
    handDetail (id,deduce) {
      this.$router.push({
        name: 'malltHomeDetail',
        query: {
          title: id,
          
        }
      })
    }
  }
}
</script>

<style scoped>
  .price{
    margin-top:10px;
  }
  .ticket{
    font-size:3vw !important;
    color:orangered !important;
  }
</style>
