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
        <div class="content_top" @click="handAddress()" v-show="obj.length == '0'">
            点击添加默认地址
        </div>
        <div class="content_top" @click="jumpTo({name:'mallAddress'})" v-show="obj.length != '0'">
          <div class="div1">
              <div class="left_name">
                <i class="iconfont icon-wode1"></i>
                <span>收货人：{{ obj.username }}</span>
              </div>
              <div class="right_phone">
                {{obj.mobile}}
              </div>
          </div>
          <div class="div2">
            <i class="iconfont icon-locationfill"></i>
            <span>{{ obj.province + obj.city + obj.district + obj.address}}</span>
            <i class="iconfont icon-right1 left"></i>
          </div>
        </div>

        <div class="border">
          <img src="../../assets/mall/img/home/border.png" alt="">
        </div>

        <div  class="one-shop-order">
          <div class="content_main">
            <div class="list">
              <div class="mail">
                <div class="left">
                  <img v-lazy="keylist.thumb" alt="">
                </div>
                <div class="right">
                  <div class="div1">
                    {{keylist.goodsName}}
                  </div>
                  <div class="div2">
                  </div>
                  <div class="div3">
                    <div class="price_main" style="margin-top:.3rem;"><span>¥</span>{{keylist.spe_price}}</div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        
      </div>
      <div class="footer">
          <div class="footer_main">
            <div class="div1">
              <label>合计</label><span>¥</span>{{keylist.spe_price}}
            </div>
            <div class="div_btn" @click="handDetail()">立即购买</div>
          </div>
      </div>
      <!-- 新老会员弹窗 -->
      <div class="share_platform win_box" v-show="win_bug">
          <div class="fenxiang_win_bg" @click="fx_win_del()"></div>
          <div class="to_buy">
            <div class="to_buy_box_t">
              <h3>请选择新会员还是老会员！</h3>
            </div>
            <div class="to_buy_box_c">
              <div @click="newMember">新会员</div>
              <div @click="oldMember">老会员</div>
            </div>
          </div>
        </div>
      <div></div>
    </div>
</template>

<script>
import * as utils from '../../utils'
import * as apiHttp from '../../api/index'
export default {
  name: 'keyfirmOrder',
  data () {
    return {
      obj: [],
      keylist:utils.storage.get('keykeyDetail'),
      addid: '',
      win_bug: false
    }
  },
  mounted: function () {
    this.addressList()
  },
  methods: {
    fx_win_del () {
      this.win_bug = false
    },
    addressList () {
        apiHttp.addressList().then(response => {
            if (response.data.list.length>0) {
                this.obj = response.data.list[0]
                this.addid = response.data.list[0]['id']
            }
        })
    },
    handAddress () {
      this.$router.push({
        name: 'mallAddAddress'
      })
    },
    handDetail () {
      if(this.obj.length == '0'){
        this.$vux.toast.text('请添加收货地址！')
      } else{
         this.win_bug = true
      }
    },
    newMember () {
      this.jumpTo('/newRegister') 
    },
    oldMember () {
      this.jumpTo('/oldMember?address_id='+this.addid) 
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
  .share_platform{
    width: 100%;
    height: 100%;
    background:rgba(24,20,20,.5);
    z-index: 9999;
    position: absolute;
    top: 0;
    .fenxiang_win_bg{
      width: 100%;
      // height: calc(100% - 3.8rem);
      height: 100%;
    }
    .to_buy{
      background: #ffffff;
      text-align: center;
      width: 100%;
      position: fixed;
      bottom: 0;
      .to_buy_box_t{
        height: .88rem;
        border: .01rem solid #e6e6e6;
        display: flex;
        justify-content: center;
        align-items: center;
      }
      .to_buy_box_c{
        height: 2rem;
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding:0 1rem;
        div{
          width: 2rem;
          height: .88rem;
          text-align: center;
          line-height: .88rem;
          background: #df2121;
          color: #ffffff;
          border-radius: .15rem;
        }
      }
    }
  }  
</style>
