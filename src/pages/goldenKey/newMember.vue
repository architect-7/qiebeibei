<template>
    <div id="lionn">
        <div class="pay_top"> <img src="../../assets/img/gift/xizai.png" alt=""></div>
        <div class="tit1">请先下载普惠APP，注册成功后</div>
        <div class="tit2">才能购买金钥匙</div>
        <div class="box_btn">
            <div @click="xiazai()">立即下载</div>
        </div>
    </div>
</template>

<script>
export default {
  name: 'newMember',
  components: {
  },
  data () {
    return {

    }
  },
  mounted:function() {

  },
  methods: {
    xiazai(){
        let browser = api.require('webBrowser')
        browser.open({
        url: "https://app.puhui88.cn/mobile/login/download_app",
        titleBar: {
            textColor: '#333',
            bg: '#fff'
        }
        })
    }
  }

}
</script>
<style  lang="less">
#lionn{
    text-align: center;
    .pay_top{
        margin-top: 1.4rem;
        img{
            width: 1.65rem;
            height: 1.6rem;
        }
    }
    .tit1{
        margin-top: .8rem;
        font-size: .3rem;
        font-weight:500;
        color:rgba(102,102,102,1)
    }
    .tit2{
        font-size: .3rem;
        font-weight:500;
        color:rgba(102,102,102,1)
    }
    .box_btn{
        margin-top: 1.8rem;
        >div{
            margin-left: .84rem;
            width: 5.8rem;
            height: .88rem;
            background:rgba(1,194,199,1);
            box-shadow: 0 .02rem .04rem 0 rgba(1,194,199,0.2);
            border-radius: .05rem;
            font-size: .32rem;
            font-weight:500;
            color:rgba(255,255,255,1);
            line-height: .88rem;
        }
    }
}
</style>
