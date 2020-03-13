<template>
    <div class="type1">
      <div v-if="allList.length==0" class="no-sort">暂无分类</div>
      <div v-if="allList.length>0" class="headd">
        <tab>
          <tab-item 
          v-for="n in list" 
          :key="n" 
          :line-width=1
          @on-item-click="switchTab(n.id)"
          :selected="n==hea">{{ n.title }}
          </tab-item>
        </tab> 
      </div >
        <div class="olilists" v-if="allList.length>0">
          <ul>
            <li v-for="(item,index) in allList" :key="index" @click="openDetail(item)">
            <div>
                <img :src="item.thumb" alt="">
            </div>
            <p>{{item.goodsName}}</p>
            <div>
                <span>￥{{item.spe_price}}</span>
                <span></span>
            </div>
            </li>
           <!-- <li>
            <div>
                <img src="../../assets/img/oli/oliac.png" alt="">
            </div>
            <p>创意陶瓷盘子手绘果盘品盘卡通碗盘碟</p>
            <div>
                <span>￥100.00</span>
                <span>￥80.00</span>
            </div>
            </li>-->
          </ul>
        </div>
    </div>
</template>

<script>
import * as apiHttp from '../../api/index'
import { Tab, TabItem, Sticky, Divider, XButton, Swiper, SwiperItem } from 'vux'
export default {
  components: {
    Tab,
    TabItem,
    Sticky,
    Divider,
    XButton,
    Swiper,
    SwiperItem
  },
  data() {
      return {
          list:['全部','水果','衣服','油券','画家','品跑','跑车','哈哈'],
          hea:'全部',
          tab:0,
          allList:[],
          type:this.$route.query.type
      }
  },
  mounted(){
    this.getTab()
  },
  methods:{
      switchTab(id){
       apiHttp.secondList(id,this.type).then(res=>{
          if(res.code === 0){
            this.allList = res.data.list;
          }else{
            this.$vux.toast.text(res.msg)
          }
        })
      },
      openDetail(item){
        this.$router.push({
          name: 'mallUnderShopDetail',
          query: {
            id: item.id,
            type:this.type
          }
        })
      },
      getTab(){
        apiHttp.categoryList(this.$route.query.spid).then(res=>{
        if(res.code === 0){
          this.list = res.data.list;
          this.switchTab(res.data.list[0].id)
        }else{
          this.$vux.toast.text(res.msg)
        }
      })
    }, 
  }
}
</script>

<style lang="less">
.no-sort{
    width:100vw;
    height:2rem;
    line-height: 2rem;
    text-align: center;
  }
.olilists {
  width: 93%;
  background-color: #fff;
  margin: .2rem auto 0;
  border-radius:10px; 
  padding-top:.3rem;
  box-sizing: border-box; 
  
  ul {
    overflow: hidden;
    display: flex;
    justify-content: space-between;
    padding:.35rem .2rem .35rem .3rem;
    flex-wrap:wrap;
    margin-bottom: .2rem;
    li {
      float: left;
      list-style: none;
      width: 48%;
      height: 4.7rem;
      margin-left: .12rem;
      div:nth-of-type(1) {
        width: 93%;
        height: 3rem;
        background-color: #fff;
        border-radius:5px;
        margin-bottom: .2rem;
        margin-right: -.2rem;
        overflow: hidden;
        img {
          width: 100%;
          height: 100%;
        } 
      }
      div:nth-of-type(2) {
        span:nth-of-type(1) {
          color:rgba(254,29,6,1);
          font-size: .3rem;
        }
        span:nth-of-type(2) {
          font-size: .25rem;
          color:#999999;
          text-decoration: line-through;
        }
      }
      p {
        font-size: .29rem;
        line-height: .37rem;
        width: 90%;
        margin-bottom: .15rem;
        text-align: left;
      }
    }
  }
}
</style>
