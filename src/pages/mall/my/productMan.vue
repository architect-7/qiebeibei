<template>
    <div class="productMan" >
       <div class="proman-tab">
         <div :class="{'proman-active':tab==1}" @click="switchTab(1)">在售</div>
         <div :class="{'proman-active':tab==2}" @click="switchTab(2)">下架</div>
         <div :class="{'proman-active':tab==3}" @click="switchTab(3)">审核中</div>
         <div :class="{'proman-active':tab==4}" @click="switchTab(4)">已售</div>
       </div>
       <div class="proman-list" v-if="list.length>0">
         <!--在售-->
         <div class="proman-item" v-if="tab==1" v-for="(item,index) in list" :key="index">
           <div class="item-row-1">
             <div class="left" :style="{ 'background': 'url(' + item.thumb + ') no-repeat'}"> 
               <div>在售</div>
             </div>
             <div class="right">
               <div>{{item.goodsName}}</div>
               <div class="right-flex">
                 <div>￥ <span>{{item.spe_price}}</span></div>
                 <div><span v-for="(it,ind) in item.goodsTag" :key="ind" style="margin-right:0.1rem"> {{it}} </span></div>
               </div>
             </div>
           </div>
           <div class="item-row-2" >
             <div v-if="item.city"><img src="../../../assets/img/product/dingwei.png" alt="">{{item.city}} {{item.district}}</div>
             <div v-else></div>
             <div>
                <span @click="soltOut(item.id,'down')">下架</span>
                <span @click="delProduct(item.id)">删除</span>
             </div>
           </div>
         </div>
         <!--下架-->
         <div class="proman-item" v-if="tab==2" v-for="(item,index) in list" :key="index">
           <div class="item-row-1">
             <div class="left" :style="{ 'background': 'url(' + item.thumb + ') no-repeat'}"> 
               <div>下架</div>
             </div>
             <div class="right">
               <div>{{item.goodsName}}</div>
               <div class="right-flex">
                 <div>￥ <span>{{item.spe_price}}</span></div>
                 <div><span v-for="(it,ind) in item.goodsTag" :key="ind" style="margin-right:0.1rem"> {{it}} </span></div>
               </div>
             </div>
           </div>
           <div class="item-row-2" >
             <div v-if="item.city"><img src="../../../assets/img/product/dingwei.png" alt="">{{item.city}} {{item.district}}</div>
             <div v-else></div>
             <div>
                <span @click="putAway(item.id,'up')">上架</span>
                <span @click="delProduct(item.id)">删除</span>
                <span @click="editProduct(item.id)">编辑</span>
             </div>
           </div>
         </div>
         <!--已售-->
         <div class="proman-item" v-if="tab==3" v-for="(item,index) in list" :key="index">
           <div class="item-row-1">
             <div class="left" :style="{ 'background': 'url(' + item.thumb + ') no-repeat'}"> 
               <div>审核中</div>
             </div>
             <div class="right">
               <div>{{item.goodsName}}</div>
               <div class="right-flex">
                 <div>￥ <span>{{item.spe_price}}</span></div>
                 <div><span v-for="(it,ind) in item.goodsTag" :key="ind" style="margin-right:0.1rem"> {{it}} </span></div>
               </div>
             </div>
           </div>
           <div class="item-row-2" >
             <div v-if="item.city"><img src="../../../assets/img/product/dingwei.png" alt="">{{item.city}} {{item.district}}</div>
             <div v-else></div>
             <div class="check">
                <span>删除</span>
                <span>编辑</span>
             </div>
           </div>
         </div>
         <!--审核中-->
         <div class="proman-item" v-if="tab==4" v-for="(item,index) in list" :key="index">
           <div class="item-row-1">
             <div class="left" :style="{ 'background': 'url(' + item.thumb + ') no-repeat'}"> 
               <div>已售</div>
             </div>
             <div class="right">
               <div>{{item.goodsName}}</div>
               <div class="right-flex">
                 <div>￥ <span>{{item.spe_price}}</span></div>
                 <div ><span v-for="(it,ind) in item.goodsTag" :key="ind" style="margin-right:0.1rem"> {{it}} </span></div>
               </div>
             </div>
           </div>
           <div class="item-row-2" >
             <div v-if="item.city"><img src="../../../assets/img/product/dingwei.png" alt="">{{item.city}} {{item.district}}</div>
             <div v-else></div>
             <div class="check">
                <span>下架</span>
                <span>删除</span>
                <span>编辑</span>
             </div>
           </div>
         </div>
       </div>
       <div class="no-data" v-if="list.length==0">暂无数据</div>
    </div>
</template>

<script>

export default {
  name: 'productMan',
  components: {
    
  },
  data () {
    return {
      data: '',
      tab:1,
      list:[]
    }
  },
  methods: {
    switchTab(type){
      this.tab = type
      this.getList(type)
    },
    getList(type){
       this.$http.post('/mall/second-goods/second-goods-list', {
         model:'self',type:type
        }).then(res => {
           this.list = res.data.list
       })
    },
    soltOut(id,type){
      this.$http.post('/mall/second-goods/down-up-second-goods', {
         id:id,
         type:type
        }).then(res => {
           if(res.code==0){
             this.$vux.toast.text(res.msg);
             this.getList(this.tab)
           }else{
             this.$vux.toast.text(res.msg);
           }
       })
    },
    putAway(id,type){
      this.$http.post('/mall/second-goods/down-up-second-goods', {
         id:id,
         type:type
        }).then(res => {
           if(res.code==0){
             this.$vux.toast.text(res.msg);
             this.getList(this.tab)
           }else{
             this.$vux.toast.text(res.msg);
           }
       })
    },
    editProduct(id){
      this.$router.replace({
        name:'product',
        query:{
          pid:id
        }
      })
    },
    delProduct(id){
      this.$http.post('/mall/second-goods/del-second-goods', {
         id:id,
        }).then(res => {
           if(res.code==0){
             this.getList(this.tab)
           }else{
             this.$vux.toast.text(res.msg);
           }
       })
    }
  },
  mounted () {
    this.getList(1)
  }
}
</script>

<style scoped lang="less">
.productMan{
    width:100%;
    text-align: center;
    font-size:0.3rem;
    color:black;
    .proman-tab{
      display:flex;
      width:100%;
      height:1rem;
      justify-content: space-around;
      align-items:center;
      background:white;
      div{
        width:1rem;
        height:1rem;
        line-height: 1rem;
        box-sizing: border-box;
      }
    }
    .proman-list{
      .proman-item{
        width:100%;
        height:3.6rem;
        border-top:8px solid #F2F2F2;
        background:white;
        .item-row-1{
          width:100%;
          display:flex;
          padding:0.3rem;
          box-sizing: border-box;
          border-bottom:1px solid #EEEEEE;
          text-align: left;
          .left{
            width:1.8rem;
            height:1.8rem;
            background-size:1.8rem 1.8rem !important;
            border-radius:0.1rem;
            position:relative;
            div{
              width:1.8rem;
              height:0.4rem;
              color:white;
              font-weight:bold;
              background:rgba(0,0,0,0.5);
              position:absolute;
              bottom:0;
              border-bottom-right-radius: 0.1rem;
              border-bottom-left-radius: 0.1rem;
              text-align: center
            }
          }
          .right{
            width:5rem;
            height:1.8rem;
            margin-left:0.1rem;
            .right-flex{
              width:5rem;
              display:flex;
              justify-content: space-between;
              align-items: center;
              margin-top:0.2rem;
              color:#FC2229;
              div:first-child{
                span{
                  font-weight:bold;
                  font-size:0.4rem;
                }
              }
              div:last-child{
                span{
                  display:inline-block;
                  width:0.7rem;
                  height:0.35rem;
                  line-height: 0.35rem;
                  font-size:0.25rem;
                  text-align: center;
                  border-radius:3px;
                  background:#FEE1E1;
                }
              }
            }
          }
        }
        .item-row-2{
          width:100%;
          box-sizing: border-box;
          padding:0.3rem;
          display:flex;
          justify-content: space-between;
          align-items: center;
          span{
            display:inline-block;
            width:1.1rem;
            height:0.5rem;
            text-align: center;
            line-height: 0.5rem;
            border:1px solid #01C2C7;
            color:#01C2C7;
            border-radius:0.1rem;
          }
          img{
            width:0.3rem;
            height:0.3rem;
          }
          .check{
            span{
              color:#B2B2B2;
              border:1px solid #B2B2B2;
            }
          }
        }
      }
    }
}
.proman-active{
  border-bottom:2px solid #01C2C7;
  font-weight:bold;
}
.no-data{
  width:100vw;
  height:2rem;
  text-align:center;
  line-height:2rem;
}
</style>
