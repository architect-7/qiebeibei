<template>
    <div id="lionn">
       <div class="pay_top"> <img src="../../assets/img/gift/payover.png" alt=""></div>
       <div class="tit">支付完成</div>
       <div class="pay_over">
           <div @click="jumpTo('/home')">返回首页</div>
           <div @click="jumpTo('/orderLIstKey')">查看订单</div>
       </div>
    </div>
</template>

<script>
export default {
  name: 'PaySuccess',
  components: {
  },
  data () {
    return {

    }
  },
  mounted:function() {

  },
  methods: {

  }

}
</script>
<style  lang="less">
#lionn{
    text-align: center;
    .pay_top{
        margin-top: 2.88rem;
        img{
            width: 1.4rem;
            height: 1.4rem;
        }
    }
    .tit{
        font-size: .28rem;
    }
    .pay_over{
        margin-top: .5rem;
        padding: 0 .8rem;
        display: flex;
        justify-content: space-between;
        >div{
            height: .88rem;
            width: 2rem;
            line-height: .88rem;
            text-align: center;
            color: #fff;
            background: #01C2C7;
            border-radius: .6rem;
            font-size: .24rem;
        }
    }
}
</style>
