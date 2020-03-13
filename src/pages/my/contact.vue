<template>
    <div class="contact">
      <div class="t vux-1px-b">
        <span class="info">在线客服时间：{{data.opentime}}</span>
        <p><span v-html="data.customerServiceTime"></span></p>
      </div>
      <div class="way vux-1px-b">
        <p>客服渠道</p>
        <p>上班时间：<span>{{data.openday}}</span></p>
        <p>微&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;信： <span>{{data.wechat}}</span></p>
        <!--<p>电话：<a :href="'tel:' + data.customerPhone">{{data.customerPhone}}</a></p>-->
      </div>
    </div>
</template>

<script>
export default {
  name: 'contact',
  components: {},
  data () {
    return {
      data: ''
    }
  },
  methods: {
    getInfo () {
      this.$http.post('/amoy/user/service-qrcode', {},true).then(res => {
        if(res.code==0 ) {
          this.data = res.data
        }else {
          this.$vux.toast.text('未获取客服信息')
        }
      })
    }
  },
  mounted () {
    // 删除
    this.getInfo()
  }
}
</script>

<style scoped lang="less">
.contact{
  height: 100%;
  background: #fff;
  .t{
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0 .2rem;
    height: .9rem;
    .info{
      font-size: .28rem;
      color: #666;
    }
    p{
      font-size: .24rem;
      span{
        color: #999;
        font-size: .24rem;
        margin-left: .1rem;
      }
    }
  }
  .way{
    p{
      font-size: .28rem;
      color: #666;
      line-height: .4rem;
      padding: .2rem 0 .2rem .5rem;
    }
    p:first-of-type{
      padding-left: .2rem;
    }
    p:last-of-type{
      a{
        color: #a1e3bd;
      }
    }
  }
}
</style>
