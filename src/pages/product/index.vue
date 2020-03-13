<template>
    <div class="product" >
      <x-header
        slot="header"
        :left-options="{showBack: false}"
      >
        <a @click="goBack()" slot="left" class="backBox"><i class="iconfont icon-back"></i></a>
        发布宝贝
        <p style="position: absolute;right: .3rem;top: 0;color: rgba(153,153,153,1);font-size:.28rem;" @click="jumpTo('/ptext')">买卖规则</p>
      </x-header>
      <div style="height:0.8rem"></div>
      <div class="pro-img" >
       <!-- <scroller lock-y scrollbar-x>-->   
        <div style="max-width:300vw;text-align:left" v-if="imgs.length>0">
          <!--<img class="item-img" :src="item" alt="" v-for="(item,index) in imgs" :key="index" @click="delImg(index)">-->
          <div class="item-img" :style="{ 'background': 'url(' + item + ') no-repeat center center', 'background-size': '2rem 2.2rem'}" v-for="(item,index) in imgs" :key="index" @click="delImg(index)">
            <img class="pro-close" src="../../assets/img/product/guanbi.png" alt="">
          </div>
        </div>
       <!-- </scroller>-->
        <img v-else  src="../../assets/img/product/photo.png" @click="uploadImg()" alt="">
        <div @click="uploadImg()">上传商品图</div>
      </div>
      <div class="pro-box"></div>
      <div class="pro-desc">
        <div><input type="text" placeholder="宝贝标题、型号等信息" v-model="goodsName"></div>
        <div v-if="isShow">
          <textarea name="" v-model="desc" ></textarea>
        </div>
        <div class="pro-introduce" v-else>
          <div v-for="(it,ind) in params" :key="ind" v-if="it.tab==true"><span>{{it.key}}:</span><input type="text" v-model="it.value"></div>
        </div>
        <div class="pro-flex" @click="maskShow(1)">
          <div><img src="../../assets/img/product/dingwei.png"  alt=""> {{city}} {{district}}</div>
          <div>提示参数 <img src="../../assets/img/product/right1.png"  alt=""></div>
        </div>
      </div>
      <div class="pro-box"></div>
      <div class="pro-spec">
        <div class="item-flex" @click="maskShow(2)">
          <div>价格</div>
          <div>开个价 <img src="../../assets/img/product/right1.png"  alt=""></div>
        </div>
        <div class="item-flex" @click="maskShow(2)">
          <div>商品原价</div>
          <div>商品原价 <img src="../../assets/img/product/right1.png"  alt=""></div>
        </div>
        <div class="item-flex" @click="maskShow(4)">
          <div>标签</div>
          <div><span v-for="(it,ind) in goodsTag" :key="ind">{{it}} </span> <img src="../../assets/img/product/right1.png"  alt=""></div>
        </div>
      </div>
      <div class="pro-mask" v-if="mask" >
        <!--参数-->
        <div class="pro-para" v-if="type==1">
          <div>选择参数</div>
          <div class="para-flex">
            <div @click="selectTab(index)" v-for="(item,index) in params" :class="{'pro-active':item.tab}" :key="index">{{item.key}}</div>
          </div>
          <div class="para-do">
            <div @click="mask=false">取消</div>
            <div @click="isDescShow()">确定</div>
          </div>
        </div>
        <!--标签-->
        <div class="pro-para" v-if="type==4">
          <div>选择标签</div>
          <div class="para-flex">
            <div @click="pushTag(index,item.key)" v-for="(item,index) in tag" :class="{'pro-active':item.tab}" :key="index">{{item.key}}</div>
          </div>
          <div class="para-do">
            <div @click="mask=false">取消</div>
            <div @click="mask=false">确定</div>
          </div>
        </div>
        <!--价格-->
        <div class="pro-price" v-if="type==2">
          <div>想卖多少钱？</div>
          <div> <span>￥ </span> <input type="text" style="width:5rem;" v-model="spe_price"></div>
          <div class="p-row-2"><span>原价: </span><input type="text" v-model="ex_price"><span>数量: </span><input type="text" class="int-1" v-model="total"></div>
          <div>
            <span>运费 </span><input type="text" v-model="postage" :placeholder=isposter>
            <span @click="isPost()" style="display:inline-flex;width:1.5rem;align-items:center;">包邮 &nbsp;
            <img v-if="freeShipping=='1'" align="absmiddle"  src="../../assets/img/product/baoyou.png" alt="">
            <img v-if="freeShipping=='2'" align="absmiddle"  src="../../assets/img/product/wxz.png" alt="">
            </span> 
          </div>
          <img class="close-mask" @click="mask=false" src="../../assets/img/product/guanbi.png" alt="">
          <div class="pro-under" @click="mask=false">下一项</div>
        </div>
        <!--确认-->
        <div class="pro-con" v-if="type==3">
          <div class="pro-row-1">{{addGoods.head}}</div>
          <div class="pro-row-2">{{addGoods.content}}</div>
          <div class="pro-row-3">
            <div class="pro-item" @click="nowRelease('protect')">
              <div> <img src="../../assets/img/product/danbao.png" alt=""> </div>
              <div class="pro-t">{{addGoods.leftTop}}</div>
              <div>{{addGoods.leftBottom}}</div>
            </div>
            <div class="pro-item" @click="nowRelease('self')">
              <div> <img src="../../assets/img/product/zixing.png" alt=""> </div>
              <div class="pro-t">{{addGoods.rightTop}}</div>
              <div>{{addGoods.rightBottom}}</div>
            </div>
          </div>
          <div class="pro-row-4" @click="mask=false"><img src="../../assets/img/product/close.png" alt=""></div>
        </div>
      </div>
      <div class="pro-now">
        <div v-if="pid" class="now-release" @click="saveProduct('self')">保存</div>
        <div v-else class="now-release" @click="maskShow(3)">立即发布</div>
      </div>
    </div>
</template>

<script>
import { Scroller } from 'vux'
export default {
  name: 'product',
  components: {
    Scroller
  },
  data () {
    return {
      data: '',
      imgs:[],
      img:false,
      mask:false,
      type:1,
      hint:[],
      total:'',
      postage :'',
      spe_price:'',
      ex_price:'',
      goodsName:'',
      goodsDesc:'',
      addGoods:'',
      isposter:'',
      isShow:true,
      city:'',
      district:'',
      freeShipping:'2',
      thumbs:[],
      tag:[{tab:false,key:'全新'},{tab:false,key:'包邮'},{tab:false,key:'面议'}],
      goodsTag :[],
      goodType:{},
      pid:this.$route.query.pid,
      params:[{tab:false,key:'转手原因',value:''},{tab:false,key:'入手渠道',value:''},{tab:false,key:'规格尺寸',value:''},{tab:false,key:'新旧程度',value:''},{tab:false,key:'使用手感',value:''},{tab:false,key:'其他问题',value:''}],
      desc:'在这里描述一下你的宝贝的 转手原因、入手渠道、规格尺寸、新旧程度和使用感受吧'
    }
  },
  methods: {
    uploadImg(){
      var that = this;
      if(this.imgs.length>=9){
        this.$vux.toast.text("图片最大数量为9张");
        return;
      }
      api.getPicture({
            sourceType: 'album',
            encodingType: 'jpg',
            mediaValue: 'pic',
            destinationType: 'base64',
            allowEdit: true,
            quality: 100,
            targetWidth: 750,
            targetHeight: 750,
            saveToPhotoAlbum: false
         }, function(ret, err) {
            if (ret) {
                if(ret.base64Data){
                  that.$http.post('/mall/second-goods/upload', {img:ret.base64Data}).then(res => {
                    if(res.code==0){
                      if(ret.base64Data){
                         that.imgs.push(ret.base64Data)
                         that.thumbs.push(res.data.img)
                      }
                    }
                  })
                }
             }
         })
    },
    delImg(index){
      this.imgs.splice(index,1)
      this.thumbs.splice(index,1)
    },
    isPost(){
      if(this.freeShipping=='1'){
        this.freeShipping='2'
        this.postage = 0
      }else{
        this.freeShipping='1'
        this.postage = '包邮'
      }
    },
    getConfig(){
       this.$http.post('/mall/second-goods/config', {}).then(res => {
         this.addGoods = res.data.addGoods
         this.goodsType = res.data.addGoods.type
         for(var i=0;i<this.params.length;i++){ 
           this.$set(this.params,this.params[i].key,res.data.addGoods.type[i+1])
         }
       })
       var that = this;
       var bMap = api.require('bMap');
        bMap.getLocation({
            accuracy: '100m',
            autoStop: true,
            filter: 1
        }, function(ret, err) {
            if (ret.status) {
               bMap.getNameFromCoords({
                    lon: ret.lon,
                    lat: ret.lat
                }, function(ret, err) {
                    if (ret.status) {
                        that.city = ret.city
                        that.district = ret.district
                    }
                });
            } else {
                
            }
        });
    },
    pushTag(index,key){
      this.tag[index].tab=!this.tag[index].tab
      this.goodsTag = [];
      for(var i=0;i<this.tag.length;i++){
        if(this.tag[i].tab){
          this.goodsTag.push(this.tag[i].key)
        }
      }
    },
    isDescShow(){
      for(var i=0;i<this.params.length;i++){
        if(this.params[i].tab){
          this.isShow = false
        }
      }
      this.mask = false
    },
    saveProduct(type){
      this.$http.post('/mall/second-goods/update-second-goods', {
        id:this.$route.query.pid,
        type:'save',
        model : type,
        goodsName : this.goodsName,
        goodsDesc : this.desc,
        spe_price : this.spe_price,
        ex_price : this.ex_price,
        freeShipping : this.freeShipping,
        postage : this.postage=='包邮'?'0.00':this.postage,
        thumb : this.thumbs[0],
        thumbs: this.thumbs,
        total : this.total,
        city:this.city,
        district:this.district,
        goodsTag:this.goodsTag
      }).then(res => {
        if(res.code==0){
          this.$vux.toast.text(res.msg)
          this.$router.replace({
            name:'mallMy'
          })
        }else{
          this.$vux.toast.text(res.msg)
        }
      })
    },
    getProductDetail(){
      this.$http.post('/mall/second-goods/update-second-goods', {
        id:this.$route.query.pid
      }).then(res => {
        var item = res.data.list
        if(item.thumbs[0]!==''){
          this.imgs = item.thumbs
          this.thumbs = item.thumbs
        }else{
          this.imgs[0] = item.thumb
          this.thumbs[0] = item.thumb
        }
        
        this.desc = item.goodsDesc
        this.goodsName = item.goodsName
      })
    },
    selectTab(index){
      this.params[index].tab=!this.params[index].tab
      this.hint.push(index)
    },
    maskShow(type){
      this.mask = true
      this.type = type
    },
    nowRelease(type){
      if(!this.isShow){
        this.desc = '';
        for(var i=0;i<this.params.length;i++){
          if(this.params[i].tab){
            this.desc = this.desc+this.params[i].key+':'+this.params[i].value+' '
          }
        }
      }
      var price = +this.spe_price>0&&+this.ex_price>0
      if(!price){
        this.$vux.toast.text("价格必须大于0");
        return;
      }
      if(this.total>0){

      }else{
        this.$vux.toast.text("数量必须大于0");
        return;
      }
      
      this.$http.post('/mall/second-goods/add-goods', {
        model : type,
        goodsName : this.goodsName,
        goodsDesc : this.desc,
        spe_price : this.spe_price,
        ex_price : this.ex_price,
        freeShipping : this.freeShipping,
        postage : this.postage,
        thumb : this.thumbs[0],
        thumbs: this.thumbs,
        total : this.total,
        city:this.city,
        district:this.district,
        goodsTag:this.goodsTag
      }).then(res => {
        if(res.code==0){
          this.$router.push({
            name:'submit',
            query:{
              type:type
            }
          });
        }else{
          this.$vux.toast.text(res.msg);
        }
      })
    }
  },
  created(){
    if(this.$route.query.pid){
      this.getProductDetail()
    }
  },
  mounted () {
    this.getConfig()
    
  }
}
</script>

<style scoped lang="less">
.product{
    width:100%;
    text-align: center;
    font-size:0.3rem;
    color:black;
    .pro-img{
      width:100%;
      background:white;
      color:#01C2C7;
      font-weight:bold;
      padding-top:0.2rem;
      .item-img{
        width:2rem;
        height:2.2rem;
        margin:0.2rem;
        border-radius:0.1rem;
        position:relative;
        display:inline-block;
      }
      .pro-close{
        width:0.3rem;
        height:0.3rem;
        position:absolute;
        right:0.05rem;
        top:0.05rem;
      }
    }
    .no-img{
      width:100%;
      height:1.6rem;
      text-align:center;
      background:white;
      padding-top:0.4rem;
      color:#999999;
      img{
        width:0.7rem;
        height:0.6rem;
      }
    }
    .pro-box{
      width:100%;
      height:0.15rem;
      background:rgb(245,245,245);
    }
    .pro-desc{
      width:100%;
      padding:0.3rem;
      box-sizing: border-box;
      color:#999999;
      background:white;
      .pro-introduce{
        width:100%;
        padding:0.3rem;
        box-sizing: border-box;
        div{
          display:flex;
          justify-content: space-between;
          align-items: center;
        }
        span{
          display:inline-block;
          width:1.5rem;
        }
        input{
          border:0;
          outline:none;
          width:5rem;
          display:inline-block;
        }
      }
      .pro-flex{
        width:100%;
        height:0.6rem;
        display:flex;
        justify-content: space-between;
        align-items: center;
        div:first-child{
          color:#333333;
        }
        img{
          width:0.25rem;
          height:0.25rem;
        }
      }
      textarea{
        resize:none;
        outline:none;
        width:100%;
        min-height:2rem;
        border:0px;
        color:#999999;
        font-size:0.3rem;
      }
      input{
        width:100%;
        height:1rem;
        border:0px;
        outline:none;
        font-weight:bold;
        font-size:0.3rem;
      }
    }
    .pro-now{
      width:100%;
      height:1.4rem;
      position:fixed;
      background:white;
      left:0;
      bottom:0;
      .now-release{
        width:7rem;
        height:1rem;
        margin:0.2rem auto;
        background:#01C2C7;
        border-radius:0.1rem;
        text-align:center;
        line-height: 1rem;
        color:white;
        font-weight:bold;
      }
    }
    .pro-spec{
      margin-bottom:1.4rem;
      .item-flex{
        width:100%;
        height:1.2rem;
        display:flex;
        justify-content: space-between;
        align-items: center;
        padding:0.3rem;
        box-sizing: border-box;
        background:white;
        border-bottom:1px solid rgb(245,245,245);
        img{
          width:0.3rem;
          height:0.3rem;
        }
      }
    }
    .pro-mask{
      width:100%;
      height:100%;
      position:fixed;
      top:0;
      left:0;
      z-index:999;
      background:rgba(0,0,0,0.5);
      .pro-para{
        width:100%;
        height:4.7rem;
        background:white;
        position:absolute;
        left:0;
        bottom:0;
        div:first-child{
          color:#333333;
          text-align: center;
          height:1.2rem;
          line-height: 1.2rem;
        }
        .para-flex{
          width:100%;
          padding:0.3rem;
          box-sizing: border-box;
          display:flex;
          flex-wrap: wrap;
          justify-content: space-between;
          align-items: center;
          div{
            width:1.8rem;
            height:0.6rem;
            text-align: center;
            line-height: 0.6rem;
            border-radius:0.1rem;
            background:#F7F7F7;
            margin-bottom:0.3rem;
            color:#666666;
            border:1px solid #F7F7F7;
          }
        }
        .para-do{
          width:100%;
          display:flex;
          justify-content: space-around;
          align-items: center;
          div{
            width:2.5rem;
            height:0.8rem;
            text-align: center;
            line-height:0.8rem;
            border-radius:0.1rem;
          }
          div:first-child{
            border:1px solid #01C2C7;
            color:#01C2C7;
          }
          div:last-child{
            background: #01C2C7;
            color:white;
          }
        }
      }
      .pro-price{
        width:100%;
        height:5.8rem;
        background:white;
        position:absolute;
        left:0;
        bottom:0;
        text-align:left;
        box-sizing: border-box;
        padding:0.3rem;
        border-top-right-radius: 0.2rem;
        border-top-left-radius: 0.2rem;
        .p-row-2{
          .int-1{
            width:2rem;
          }
        }
        .pro-under{
          width:4rem;
          height:0.8rem;
          text-align: center;
          line-height: 0.8rem;
          border-radius:0.1rem;
          margin:0.4rem auto;
          background:#01C2C7;
          color:white;
        }
        .close-mask{
          position:absolute;
          right:0.2rem;
          bottom:5.3rem;
          width:0.3rem;
          height:0.3rem;
        }
        div{
          height:1rem;
          line-height: 1rem;
          border-bottom:1px solid rgb(235,235,235);
          img{
            width:0.3rem;
            height:0.3rem;
          }
        }
        div:first-child{
            border:0;
        }
        input{
          border:0;
          outline:none;
          font-size:0.25rem;
          width:3rem;
        }
      }
      .pro-con{
        width:100%;
        height:4.7rem;
        background:white;
        position:absolute;
        left:0;
        bottom:0;
        text-align:left;
        box-sizing: border-box;
        padding:0.3rem;
        border-top-right-radius: 0.2rem;
        border-top-left-radius: 0.2rem;
        color:#333333;
        .pro-row-1{
          font-size:0.35rem;
          font-weight:bold;
          margin-bottom:0.1rem;
        }
        .pro-row-3{
          width:100%;
          display:flex;
          justify-content: space-around;
          align-items: center;
          margin-top:0.3rem;
          .pro-item{
            width:3rem;
            text-align: center;
          }
          img{
            width:1.2rem;
            height:1.2rem;
          }
          .pro-t{
            font-weight:bold;
            color:#333333;
          }
          div:last-child{
            color:#999999;
          }
        }
        .pro-row-4{
          width:100%;
          text-align:center;
          img{
            width:0.4rem;
            height:0.4rem;
            margin-top:0.2rem;
          }
        }
      }
    }
}
.pro-active{
  background:#FFF3F3 !important;
  border:1px solid #FF1D1C !important;
  color:#FF1D1C !important;
}
input{
  height:0.9rem;
  line-height: 0.5rem;
}
</style>
