<template>
  <div class="home_list" style="background:#F7F7F7" v-if="list">
    <div class="header_count">
      <ul>
        <li v-for="(item,index) in list"  @click="handDetail(item)" :key="index">
          <img v-lazy="item.thumb" alt="">
          <div class="shop_name">
            <table></table>
            <p>{{item.goodsName}}</p>
            <!--<div class="market-price">
              <del>¥ <span>{{item.marketprice}}</span></del>
            </div>-->
            <div class="price">
              <div class="price_main" style="font-size:0.25rem;">¥ <span>{{item.spe_price}}</span></div>
              <span style="color:#FF1D1C;font-size:0.2rem;" class="tag" v-for="(it,ind) in item.goodsTag" :key="ind">{{it}}</span>
            </div>
          </div>
          <div class="sell-msg">
            <div class="sell-left"><img :src="item.userThumb?item.userThumb:'./static/img/auth/logo.png'" alt=""> </div>
            <div class="sell-right">
              <div>{{item.userName?item.userName:'暂无昵称'}}</div>
              <div class="">{{item.district?item.district:'暂无地区'}} | {{item.updated_at.slice(5)}}</div>
            </div>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'list',
  props: ['list'],
  methods: {
    handDetail (item) {
      this.$router.push({
        name: 'mallUnderShopDetail',
        query: {
          id: item.id,
          type:'protect'
        }
      })
    }
  }
}
</script>

<style scoped lang="less">
    .price{
      padding-bottom:0.2rem;
      border-bottom:1px solid rgb(240,240,240);
    }
    .price>.tag{
        display: inline-block;
        width: 0.5rem;
        height: 0.35rem;
        line-height: 0.35rem;
        text-align: center;
        border-radius: 3px;
        background: #FEE1E1;
        color:#FF1D1C;
    }
    .sell-msg{
      display:flex;
      width:100%;
      padding: 0.15rem;
      padding-top:0.2rem;
      padding-bottom:0px;
      box-sizing: border-box;
      .sell-left{
        img{
          width:0.7rem;
          height:0.7rem;
          border-radius:50%;
        }
      }
      .sell-right{
        width:2.8rem;
        overflow: hidden;/*超出部分隐藏*/
        text-overflow:ellipsis;/* 超出部分显示省略号 */
        white-space: nowrap;/*规定段落中的文本不进行换行 */
        div:first-child{
          color:black;
        }
        div:last-child{
          color:#666666
        }
      }
    }
</style>
