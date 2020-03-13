<template>
    <div id="lionn">
        <div class="mescrollBox">
            <mescroll-vue  ref="mescroll" :down="mescrollDown" :up="mescrollUp" @init="mescrollInit">
                <div id="d9">
                   <div class="core">
                        <div class="core_box" v-for="(item,index) in shopList" :key="index">
                            <div class="core_t">
                                <div style="color:#df2121">{{item.turn_reward}}</div>
                                <div>订单号：{{item.created_at}}</div>
                            </div>
                        </div>
                   </div>
                </div>
            </mescroll-vue>
        </div>
    </div>
</template>

<script>
import * as apiHttp from '../../api/index'
import MescrollVue from 'mescroll.js/mescroll.vue'
export default {
  name: 'luckList',
  components: {
      MescrollVue
  },
  data () {
    return {
      mescroll: null,
      mescrollDown: {
        auto: false,
        callback: this.refresh
      },
      mescrollUp: {
        callback: this.upCallback,
        page: {
          num: 0
        },
        htmlNodata: '<p class="upwarp-nodata">—— 我也是有底线的 ——</p>',
        noMoreSize: 5,
        toTop: {
          src: './static/img/auth/back_top.png',
          offset: 1000
        },
        empty: {
          warpId: 'd9',
          icon: './static/img/auth/wudingdan.png',
          tip: '暂无相关订单~'
        }
      },
      shopList:[]
    }
  },
  mounted:function() {

  },
  methods: {
      mescrollInit (mescroll) {
      this.mescroll = mescroll
    },
    upCallback (page, mescroll) {
      this.$http.post('/mall/gold/luck-list',{page:page.num}, true, true).then((res) => {
        if (res.code === 0) {
          let arr = res.data.list === '' ? [] : res.data.list
          if (page.num === 1) this.shopList = []
          this.shopList = this.shopList.concat(arr)
        console.log(this.shopList)
          this.$nextTick(() => {
            mescroll.endSuccess(res.data.list.length, true)
          })
        }
      }).catch((e) => {
        mescroll.endErr()
      })
    },
  }

}
</script>
<style  lang="less">
#lionn{
    .mescrollBox{
      width: 100%;
      position: absolute;
      top: .88rem;
      bottom: 0;
      height: auto !important;
    }
    .core{
        padding: 0 .2rem;
        .core_box{
            margin-top: .2rem;
            background: #fff;
            border-radius: .15rem;
            .core_t{
                height: .6rem;
                padding: .1rem .3rem;
                display: flex;
                align-items: center;
                justify-content: space-between;
            }
        }
    }
}
</style>
