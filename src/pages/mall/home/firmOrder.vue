<template>
    <div class="firmOrder_main">
        <x-header
          slot="header"
          :left-options="{showBack: false}"
        >
          <a @click="goBack()" slot="left" class="backBox"><i class="iconfont icon-back"></i></a>
          确认订单
        </x-header>
      <div class="content">
        <div class="content_top" @click="handAddress()" v-show="obj.address == ''">
            点击添加默认地址
        </div>
        <div class="content_top" @click="jumpTo({name:'mallAddress'})" v-show="obj.address">
          <div class="div1">
              <div class="left_name">
                <i class="iconfont icon-wode1"></i>
                <span>收货人：{{obj.address ? obj.address.username : ''}}</span>
              </div>
              <div class="right_phone">
                {{obj.address ? obj.address.mobile : ''}}
              </div>
          </div>
          <div class="div2">
            <i class="iconfont icon-locationfill"></i>
            <span>{{obj.address ? obj.address.province + obj.address.city + obj.address.district + obj.address.address: ''}}</span>
            <i class="iconfont icon-right1 left"></i>
          </div>
        </div>
        <div class="border">
          <img src="../../../assets/mall/img/home/border.png" alt="">
        </div>
        <div v-for="(items,index) in obj.cartList" :key="index" class="one-shop-order">
          <div class="content_main">
            <div class="list">
              <div class="header">
                <img v-lazy="items.shopInfo.logo" alt="">
                <span>{{items.shopInfo.title}}</span>
              </div>
              <div class="mail" v-for="(item,index) in items.goods" :key="index">
                <div class="left">
                  <img v-lazy="item.thumb" alt="">
                </div>
                <div class="right">
                  <div class="div1">
                    {{item.goodsName}}
                  </div>
                  <div class="div2">
                    <div>
                    {{item.fieldInfo}}
                    </div>
                    <div>
                      ×{{item.goodsNum}}
                    </div>
                  </div>
                  <div class="div3">
                    <div class="price_main"><span>¥</span>{{item.goodsPrice}}</div>
                  </div>
                </div>
              </div>
              <div class="li_main">
                <div class="title_left">快递费</div>
                <div class="title_right">{{items.express.expressName}}</div>
              </div>
              <div class="li_main" v-if="items.express.postagePrice > 0">
                <div class="title_left">运费</div>
                <div class="title_right_red">{{items.express.postagePrice}}</div>
              </div>
            </div>
          </div>
          <div class="content_main" style="margin-top:0.1rem;">
            <div class="list">
              <div class="li_main li_main2">
                <div class="title_left">卖家留言</div>
                <div class="title_right" style="width: 80%"><input type="text" placeholder="（选填），不超过50字" v-model="items.models" style="width: 100%;text-align: left"></div>
              </div>
              <div class="li_main">
                <div class="title_left">商品总计</div>
                <div class="title_right_red">
                  <span>¥</span>{{items.shopInfo.countPrice}}
                </div>
              </div>
              <div class="li_main" @click="selectCoupon()" v-show="!groupType">
                <div class="title_left">优惠券</div>
                <div class="title_right_red">
                  <span style="color:#a1a1a1">{{coupon}} </span><i class="iconfont icon-right1 left"></i>
                </div>
              </div>
              <!--类型选择-->
              <!--<div class="type_sel">
                <div>购买油卡请选择类型</div>
                <div>
                 <select v-model="type" style="border-radius:5px;outline:none;border:1px solid gray;">
                  <option value="选择"> 选择</option>
                  <option  v-for="(item,index) in oilCard" :value ="item" :key="index">{{item}}</option>
                 </select>
                </div>
              </div>-->
            </div>
          </div>
          <div class="content_main" v-if="items.shopInfo.deduce" style="margin-top:0.1rem;">
            <div class="list jifen-box">
              <div class="line">
                <img src="../../../assets/mall/img/jifen.png">
                <p>可用积分: {{availableJifen}}</p>
                <img v-if="!items.useJifen" src="../../../assets/img/balance/weixuan.png" @click="changeJifen(items, 1)">
                <img src="../../../assets/img/balance/yixuan.png" v-else  @click="changeJifen(items, 0)">
              </div>
              <div v-if="items.shopInfo.deduce.type!=3" style="padding-left: 0.5rem; color: #999999;">*{{items.shopInfo.deduce.info}}</div>
              <input v-else type="number" placeholder="请输入积分数量" v-model="items.inputJifenNum" @blur="changeJifen(items, items.inputJifenNum)">
            </div>
          </div>
        </div>
      </div>
      <div class="footer">
          <div class="footer_main">
            <div class="div1">
              <label>合计</label><span>¥</span>{{sumPrice}}
            </div>
            <div class="div_btn" @click="handDetail()">立即购买</div>
          </div>
      </div>
      <div  class="coupon-mask" v-if="mask">
        <div class="coupon-box">
          <div class="coupon-tit" @click="mask=false">优惠券</div>
          <div class="coupon-line"></div>
          <div class="coupon-list">
            <div class="coupon-item" v-for="(item,index) in couponList" :key="index" @click="confirmCoupon(item)">
              <div class="coupon-left">
                <div class="coupon-left-1">￥ <span>{{item.sub}}</span></div>
                <div class="coupon-left-2">满{{item.full}}可用</div>
              </div>
              <div class="coupon-right" >
                <div>{{item.types}}</div>
                <div>{{item.created_at}}</div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
</template>

<script>
import * as apiHttp from '../../../api/index'
export default {
  name: 'firmOrder',
  data () {
    return {
      obj: {},
      aJifen: 0,
      oilCard:[],
      type:"选择",
      mask:false,
      coupon:'请选择',
      couponid:'',
      couponList:[],
      GroupId:this.$route.query.GroupId,
      groupType:this.$route.query.groupType,
      addGroup:this.$route.query.addGroup,
      totalPrice: 0, // 本次订单总价
      jifenRate: 0 // 每元有多少积分
    }
  },
  mounted: function () {
    this.getCoupon()
    // 删除
    if (this.$route.query.type) {
      this.getDetail()
    } else {
      this.getDetail('interim', this.$route.query.data)
    }
  },
  computed: {
    availableJifen: {
      get () {
        // 计算可用积分
        let availabel = this.obj.credit1
       
        for (let i = 0, len = this.obj.cartList.length; i < len; i++) {
          if (this.obj.cartList[i].useJifen) {
            if(!this.obj.cartList[i].useJifenNum){
              //this.$vux.toast.text("可用积分不足");
            }else{
              availabel = availabel - this.obj.cartList[i].useJifenNum
            }
          }
        }
        return availabel
      }
    },
    sumPrice () {
      // 计算合计
      let sumPrice = +this.obj.countPrice
        
      for (let i = 0, len = this.obj.cartList.length; i < len; i++) {
        if (this.obj.cartList[i].useJifen) {
          if(!this.obj.cartList[i].useJifenNum){
            //this.$vux.toast.text("可用积分不足");
          }else{
            sumPrice = sumPrice - this.obj.cartList[i].useJifenNum / this.jifenRate
          }
          
        }
      }
      
      return sumPrice
    }
  },
  methods: {
    // 切换使用积分时调用
    changeJifen (items, val) {
      items.useJifen = val
      if (items.shopInfo.deduce.type == 3) {
        if (!items.inputJifenNum) {
          // 刚开始此值为undefine
          items.inputJifenNum = 0
        }
        // 当抵扣类型为用户输入时，要做验证，数字输入不能过大
        if (items.inputJifenNum < items.useJifenNum) {
          // 当减少数量时
          this.availableJifen = this.availableJifen + (items.useJifenNum - items.inputJifenNum)
        } else {
          // 当增加数量时
          if (+val > +this.availableJifen) {
            // 当数字超过已有积分数量
            items.inputJifenNum = +this.availableJifen + +items.useJifenNum
          }
          if (+items.inputJifenNum / this.jifenRate > this.obj.countPrice) {
            // 当积分价值超过商品价值
            items.inputJifenNum = this.obj.countPrice * this.jifenRate
          }
        }
        items.useJifenNum = items.inputJifenNum
      } else {
        // 当抵扣类型为固定积分抵扣时
       if (val && +this.availableJifen < +items.shopInfo.deduce.credit1) {
          // 当要开启此积分抵扣但是可用积分不足时
          items.useJifen = 0
          //this.$vux.toast.text('可用积分不足')
        }
      }
    },
    getDetail (type, data) {
      apiHttp.cardCheckout(type, data,this.groupType).then(response => {
        if (response.code === 0) {
          this.obj = response.data
          this.jifenRate = this.obj.creditConfig.credit3.exchange.credit1 / 100
          let list = this.obj.cartList
          this.oilCard = response.data.oilCard;

          for (let i = 0; i < list.length; i++) {
            list[i].models = ''
            this.$set(list[i], 'useJifen', 0)
            if (list[i].shopInfo.deduce && list[i].shopInfo.deduce.type != 3) {
              // 当可以使用积分，并且是固定积分时
              this.$set(list[i], 'useJifenNum', list[i].shopInfo.deduce.credit1)
            } else {
              // 当不可使用积分或者不是固定积分时
              this.$set(list[i], 'useJifenNum', '')
            }
          }
        }
      })
    },
    handAddress () {
      this.$router.push({
        name: 'mallAddAddress'
      })
    },
    selectCoupon(){
       this.mask = true
    },
    confirmCoupon(item){
      if(item.goodsId==0||item.goodsId=='0'){
        if(+this.sumPrice>+item.full){
          this.mask = false
          this.coupon = item.sub
          this.couponid = item.coupon_id
        }else{
          this.$vux.toast.text("金额不符合")
        }
      }else{
        var isShop = this.$route.query.data.includes(item.goodsId)
        if(isShop){
          if(+this.sumPrice>+item.full){
            this.mask = false
            this.coupon = item.sub
            this.couponid = item.coupon_id
          }else{
            this.$vux.toast.text('该优惠券不符合使用条件')
          }
        }
      }
    },
    getCoupon(){
      this.$http.post('mall/coupon/user-coupon-list', {}, false).then(res => {
        this.couponList = res.data.list;
      })
    },
    queryOrder (data) {
      let noteList = []
      let deduceList = []
      let list = this.obj.cartList
      for (let i = 0; i < list.length; i++) {
        noteList[list[i].shopInfo.id] = list[i].models
        if (list[i].useJifen) {
          // 使用积分
          if (list[i].shopInfo.deduce.type == 3) {
            // 如果使用积分并且使用方式是用户输入时
            deduceList[list[i].shopInfo.id] = list[i].useJifenNum
          } else {
            // 否则为1
            deduceList[list[i].shopInfo.id] = 1
          }
        } else {
          // 不使用积分
          deduceList[list[i].shopInfo.id] = 0
        }
      }
      if(this.type=='选择'){
        this.type = ""
      }
      apiHttp.orderCreate(data, noteList, deduceList,this.type,this.groupType,this.addGroup,this.GroupId,this.couponid).then(response => {
        if (response.code === 0) {
          this.$router.push({
            name: 'mallPayMent',
            query: {
              orderId: response.data,
              oilCard:this.type
            }
          })
        }
      })
    },
    handDetail () {
      if (this.$route.query.type) {
        this.queryOrder()
      } else {
        this.queryOrder(this.$route.query.data)
      }
    }
  }
}
</script>

<style scoped lang="less">
  .type_sel{
    width:90vw;
    height:50px;
    display:flex;
    justify-content: space-between;
    align-items: center;
    font-size:3.5vw;
  }
  .coupon-mask{
    position: fixed;
    top:0;
    left:0;
    width:100%;
    height:100vh;
    z-index:999;
    background:rgba(0,0,0,0.4);
    .coupon-box{
      width:100%;
      height:80%;
      background:white;
      margin-top:38%;
      padding:0.3rem;
      box-sizing: border-box;
      font-size:0.25rem;
      .coupon-tit{
        font-size:0.35rem;
        font-weight:bold;
        color:black;
      }
      .coupon-line{
        width:100%;
        height:2px;
        background:rgb(235,235,235);
        opacity: 0.4;
      }
      .coupon-list{
        .coupon-item{
          width:7rem;
          height:1.5rem;
          margin:0.2rem auto;
          display:flex;
          .coupon-left{
            width:1.6rem;
            height:1.5rem;
            background:#01C2C7;
            color:white;
            border-radius:0.1rem;
            border-right:1px dotted #01C2C7;
            text-align: center;
            .coupon-left-1{
              span{
                font-size:0.5rem;
                font-weight:bold;
              }
            }
          }
          .coupon-right{
            width:5.4rem;
            height:1.5rem;
            background:white;
            border:1px dashed rgb(235,235,235);
            border-radius:0.1rem;
            text-align:center;
            div{
              margin-top:0.2rem;
            }
            div:first-child{
              font-weight:bold;
            }
            div:last-child{
              color:gray;
            }
          }
        }
      }
    }
  }
</style>
