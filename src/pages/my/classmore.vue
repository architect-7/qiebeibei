<template>
    <div class="cmore">
      <x-header :left-options="{showBack: false}"><a slot="left" @click="goBack()"  class="backBox">
        <i class="iconfont icon-back"></i></a>
        分类
        <span class="save"   slot="right" @click="moreThan()"><img align="AbsMiddle" src="../../assets/img/oil/more.png" alt=""></span>
      </x-header>
      <div class="more-box">
        <div id="more-head">
          <scroller lock-y   >
            <div class="tab-list" >
              <span :class="{'r_gray':tab==index}" v-for="(item,index) in list" :key="index" @click="switchTab(item.id,index)">{{item.title}}</span>
            </div>
          </scroller>
        </div>
        <div class="more-list" >
          <div class="left" v-for="(item,index) in prolist" :key="index" @click="openDetail(item.id)">
            <div><img v-lazy="item.thumb" alt=""></div>
            <div class="title">{{item.goodsName}}</div>
            <div class="price"><span>￥</span><span class="s-2">{{item.spe_price}} </span><span class="s-3"> ￥{{item.marketprice}}</span></div>
          </div>
        </div>
        
      </div>
      <div class="mask" v-if="mask">
        <div class="mask-box">
          <div class="left">
            <ul>
              <li :class="{'w-tab':title==index}" @click="selectTab(item.id,index)" v-for="(item,index) in Plist" :key="index">{{item.title}}</li>
            </ul>
          </div>
          <div class="right-class">
            <div class="right" v-if="item.list.length>0" v-for="(item,index) in Pcontent" :key="index" >
             <div class="flex-title" @click="switchSon(item.id)">
               <div class="active-tab">{{item.title}}</div>
               <div class="all">全部<img src="../../assets/img/oil/right.png" align="top"  alt=""></div>
             </div>
             <div class="lbox" >
               <div class="item item-p" v-for="(it,i) in item.list" :key="i" @click="switchTab(it.id,i)">
                 <div><img :src="it.thumb" alt=""></div>
                 <div class="tab-t">{{it.title}}</div>
               </div>
              </div>
             </div>
          </div>
         </div>
       </div>
    </div>
</template>
<script>
import { XHeader, Scroller  } from 'vux'
import * as apiHttp from '../../api/index'
  export default {
    name: 'cmore',
    components:{
      XHeader,
      Scroller 
    },
    data () {
      return{
        tab:0,
        title:0,
        pid:this.$route.query.id,
        mask:false,
        list:[],
        prolist:[],
        plist:[],
        Plist:[],
        img:[],
        Pcontent:[],
        content:[]
      }
    },
    mounted(){
       this.getclassify();
       
    },
    methods: {
       getclassify(){
          apiHttp.categoryList(this.pid).then(res => {
          if (res.code === 0) {
            this.list = res.data.list
            //this.prolist = res.data.list[0].list;
            this.$http.post('/mall/index/goods-list', {cid:res.data.list[0].id}).then(res => {
                this.prolist = res.data.list
            })
          }
        })
       },
       switchTab(id,index){
         if(this.mask){
           this.mask = false
         }
         this.tab = index
         this.$http.post('/mall/index/goods-list', {cid:id}).then(res => {
              this.prolist = res.data.list
          })
       },
       switchSon(id){
          this.pid = id;
          this.mask = false
          apiHttp.categoryList(this.pid).then(res => {
          if (res.code === 0) {
            this.list = res.data.list
            //this.prolist = res.data.list[0].list;
            this.$http.post('/mall/index/goods-list', {cid:res.data.list[0].id}).then(res => {
                this.prolist = res.data.list
            })
           }
          })
       },
       selectTab(id,index){
         this.title = index;
         apiHttp.categoryList(id).then(res => {
          if (res.code === 0) {
             this.Pcontent = res.data.list;
           }
         })
       },
       openDetail(id){
         this.$router.push({
          name: 'mallHomeDetail',
          query: {
            title: id
          }
        })
       },
       moreThan(){
         this.mask = !this.mask;
         apiHttp.indexCategory().then(res => {
          if (res.code === 0) {
            this.img = res.data.list[0].thumb
            this.Plist = res.data.list
            this.Pcontent = res.data.category
          }
        })
       }
    }
  }
</script>
<style  >
  .cmore{
    width:100vw;
  }
  .cmore>.more-box{
    margin-top:12vw;
  }
  .cmore .save>img{
    width:4.5vw;
    height:4.5vw;
  }
  .cmore .tab-list{
    width:200vw;
  }
  #more-head{
    width:200vw;
    position:absolute;
    top:13vw;
    left:0;
    height:8vw;
    line-height: 8vw;
    background:rgb(235,235,235)
  }
  #more-head .tab-list>span{
    display:inline-block;
    width:20vw;
    text-align: center;
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
  }
  .r_gray{
    color:#01C2C7;
  }
  .more-box>.more-list{
    width:96vw;
    padding:2vw;
    padding-top:10vw;
    display:flex;
    justify-content: space-between;
    align-items: center;
    flex-wrap: wrap
  }
  .more-box>.more-list>div{
    width:47vw;
    margin-bottom:2vw;
    margin-top:2vw;
    background:white
  }
  .more-box>.more-list>.left>.title{
    width:47vw;
    font-weight:4.5vw;
    font-weight:bold;
    overflow: hidden;
    display: -webkit-box;
    text-overflow:ellipsis;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    color:rgb(51,51,51)
  }
  .more-box>.more-list>.left>.price{
    color:red;
    font-size:3.5vw;
  }
  .more-box>.more-list>.left>.price>.s-2{
    font-weight:bold;
    font-size:5vw;
  }
  .more-box>.more-list>.left>.price>.s-3{
    color:rgb(151,151,151)
  }
  .cmore>.mask{
    width:100vw;
    height:90vh;
    background:rgb(0,0,0,.5);
    position:absolute;
    top:12vh;
    left:0;
  }
  .mask>.mask-box{
    width:100vw;
    min-height: 70vh;
    background:white
  }
  .mask>.mask-box>div{
    display:inline-block;
  }
  .mask>.mask-box>.left{
    width:24vw;
    height:70vh;
    border-right-style:solid;
    border-right-width: 1px;
    text-align:center;
    border-right-color: rgb(235,235,235)
  }
  .mask>.mask-box>.left>ul>li{
    height:11vw;
    line-height: 11vw;
  }
  .mask>.mask-box>.right-class{
    width:74vw;
    position:absolute;
    right:0;
    top:0
  }
  .mask>.mask-box>.right{
    width:74vw;
    right:0;
    top:0
  }
  .mask>.mask-box .right>.lbox{
    padding:4vw;
    padding-top:2vw;
    /*display:flex;
    justify-content: space-between;
    align-items: center;*/
    
  }
  .mask>.mask-box .right>.lbox .item-p{
    display:inline-block;
    margin-right:4vw;
  }
  .mask>.mask-box .right>.lbox .tab-t{
    text-align: center
  }
  .mask>.mask-box .right>.lbox img{
    width:13vw;
    height:13vw;
  }
  .mask>.mask-box .right>.flex-title{
    padding:3vw;
    padding-top:4vw;
    display:flex;
    justify-content: space-between;
    align-items: center
  }
  .mask>.mask-box .right>.flex-title .all{
    color:rgb(151,151,151)
  }
  .mask>.mask-box .right>.flex-title .all>img{
    width:4.5vw;
    height:4.5vw;
  }
  .mask>.mask-box .right>.flex-title>.active-tab{
    font-size:3.5vw;
    color:rgb(51,51,51);
    font-weight:bold
  }
  .w-tab{
    color:#01C2C7;
    font-weight:bold;
    font-size:4vw;
  }
</style>

