<template>
    <div class="submit" >
      <div class="submit-msg">
          <div><img src="../../assets/img/product/success.png" alt=""></div>
          <div class="sumit-m">提交成功</div>
          <div>请耐心等待平台审核</div>
          <div class="submit-flex">
              <div @click="jumpTo('/home')">返回首页</div>
              <div @click="jumpTo('/mall/productMan')">查看订单</div>
          </div>
      </div>
      <div class="exchange-tip" v-if="type=='protect'">
          <div><span class="span-1"></span>&nbsp; 交易提醒</div>
      </div>
      <div class="order-msg" v-if="type=='protect'">
          <div class="return-msg">{{protect.content}}</div>
          <div class="order-desc">
              <div class="left">
                  <div>{{protect.name}}   {{protect.phone}}</div>
                  <div>{{protect.Provinces}} {{protect.address}}</div>
              </div >
              <div class="right" @click="onCopy(protect.name+protect.phone+protect.Provinces+protect.address)">复制地址</div>
          </div>
      </div>
    </div>
</template>

<script>

export default {
  name: 'submit',
  components: {},
  data () {
    return {
      data: '',
      protect:'',
      type:this.$route.query.type
    }
  },
  methods: {
      getConfig(){
       this.$http.post('/mall/second-goods/config', {}).then(res => {
           this.protect = res.data.addGoods.protect;
       })
    },
     onCopy (orderSn) {
      let that = this
      let clipBoard = api.require('clipBoard')
      clipBoard.set({
        value: orderSn
      }, function (ret, err) {
        if (ret) {
          that.$vux.toast.text('复制成功')
        } else {
          that.$vux.toast.text('请重试')
        }
      })
    },
  },
  mounted () {
     this.getConfig()
  }
}
</script>

<style scoped lang="less">
.submit{
    font-size:0.3rem;
    color:#333333;
    width:100%;
    text-align: center;
    .submit-msg{
        width:100%;
        background:white;
        .sumit-m{
        font-weight:bold;
        margin-top:0.4rem;
        margin-bottom:0.2rem;
        }
        img{
            width:1.2rem;
            height:1.2rem;
            margin-top:0.8rem;
        }
        .submit-flex{
            width:100%;
            display:flex;
            justify-content: space-around;
            margin-top:0.8rem;
            padding-bottom:0.5rem;
            div{
                width:2.4rem;
                height:0.8rem;
                border-radius:0.1rem;
                text-align: center;
                line-height:0.8rem;
            }
            div:first-child{
                border:1px solid #01C2C7;
                color:#01C2C7;
            }
            div:last-child{
                background:#01C2C7;
                color:white;
            }
        }
    }
    .exchange-tip{
        width:100%;
        padding:0.3rem;
        box-sizing: border-box;
        color:#333333;
        font-size:0.32rem;
        font-weight:bold;
        text-align:left;
        .span-1{
            width:2px;
            height:0.15rem;
            border-radius:3px;
            border:2px solid#01C2C7;
        }
    }
    .order-msg{
        width:100%;
        height:1.6rem;
        height:5rem;
        text-align: left;
        background:white;
        .return-msg{
            width:7rem;
            margin:0 auto;
            padding-top:0.2rem;
        }
        .order-desc{
            width:7rem;
            background:#F8F8F8;
            border-radius:0.1rem;
            margin:0.2rem auto;
            padding:0.2rem;
            box-sizing: border-box;
            display:flex;
            justify-content: space-between;
            align-items: center;
            .left{
                font-size:bold;
                color:#333333;
                width:5rem;
                div:first-child{
                    font-weight:bold;
                }
                div:last-child{
                    color:#666666;
                    margin-top:0.1rem;
                }
            }
            .right{
                width:0.8rem;
                height:1.2rem;
                border-radius:0.1rem;
                background:#FFE9E9;
                text-align: center;
                line-height: 0.6rem;
                color:#FF1D1C;
                border:1px dotted #FF1D1C;
            }
        }
    }
}
</style>
