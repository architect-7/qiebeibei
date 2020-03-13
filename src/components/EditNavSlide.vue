<template>
    <div class="EidtNavSlide"   :style="{backgroundColor: data.bgColor}">
      <swiper class="banner_list" :options="swiperOption1">
        <swiper-slide class="allBox">
      <div class="banner_list">
        <div class="allBox">
          <div class="allList" v-for="(item,index) in data " :key="index"  >
            <div  v-for="(it,ind) in item" :key="ind" style="width:1.15rem;text-align:center;margin-bottom:0.2rem;" @click="navLink(it)">
              <img v-lazy="it.thumb" alt="" :key="it.thumb">
              <p >{{it.title}}</p>
            </div>
          </div>
       </div>
      </div>
       </swiper-slide>
      </swiper>
      <div class="swiper-scrollbar">
      </div>
    </div>
</template>

<script>
import {swiper, swiperSlide} from 'vue-awesome-swiper'
import 'swiper/dist/css/swiper.css'
export default {
  name: 'EidtNavSlide',
  components: {
    swiper,
    swiperSlide
  },
  props: ['data'],
  data () {
    return {
      swiperOption1: {
        slidesPerView: 6,
        freeMode: true,
        scrollbar: {
          el: '.swiper-scrollbar'
        }
      }
    }
  },
  mounted(){
    
  },
  methods: {
    navLink(item){
      if(item.goodsId){
       this.$router.push({
          name: 'mallHomeDetail',
          query: {
            title: item.goodsId
          }
        })
      }else if(item.shopsId){
        this.$router.push({
          name: 'mallSeller',
          query: {
            spid: item.shopsId,
            type: 1
          }
        })
      }else if(item.goodsCid){
        this.$router.push({
          name: 'mallProductList',
          query: {
            cid: item.goodsCid,
            keyword: item.title
          }
        })
      }else{
        this.$router.push(item.types)
      }
    }
  }
}
</script>

<style  lang="less">
  .EidtNavSlide{
    margin-top: -1px;
    width:100vw;
    .banner_list{
      box-sizing: border-box;
      .allBox{
        display: flex;
        width:100vw;
        .allList{
          width:100%;
          margin-bottom: 0.2rem;
          text-align: center;
          justify-content: space-between;
          flex-wrap: wrap;
          font-size:0.26rem;
          margin-bottom:0.1rem;
          padding: 0 0.23rem;
          width:1rem!important;
          img{
            width:0.84rem;
            height:0.84rem;
          }
          p{
            color:#666666;
            width: 1.2rem;
          }
        }
      }
    }
    .swiper-scrollbar{
      height: 0.04rem;
      width: 12%;
      margin: 0 auto;
      background: #E4E4E4;
      div:first-child{
        background-color: #999999;
        width: 50%;
      }
    }
  }
</style>
