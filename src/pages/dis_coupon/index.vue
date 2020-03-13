<template>
    <div class="group-pur" >
        <div class="list">
            <mescroll-vue ref="mescroll" :down="mescrollDown" :up="mescrollUp" @init="mescrollInit" :style="{overflowY: showMore ? 'hidden' : 'auto'}">
                <coupon-list :list="dataList"></coupon-list>
            </mescroll-vue>
        </div>
    </div>
</template>

<script>
import MescrollVue from 'mescroll.js/mescroll.vue'
import couponList from '../../components/couponList'
export default {
  name: 'dis_coupon',
  components: {
    MescrollVue,
    couponList
  },
  data () {
    return {
      data: '',
      dataList:[],
      mescroll: null,
      mescrollDown: {
        auto: false,
        callback: this.refresh
      },
      showMore:'',
      mescrollUp: {
        user: false,
        callback: this.upCallback,
        page: {
          num: 0
        },
        scrollbar: {
          user: false
        },
        htmlNodata: '<p class="upwarp-nodata">-- END --</p>',
        htmlLoading: '<p class="upwarp-progress mescroll-rotate"></p><p class="upwarp-tip">加载中..</p>',
        noMoreSize: 5,
        toTop: {
          src: './static/img/auth/back_top.png',
          offset: 1000
        },
        empty: {
          warpId: 'empty',
          icon: './static/img/auth/kong.png',
          tip: '暂无相关数据~'
        }
      },
    }
  },
  methods: {
    mescrollInit (mescroll) {
      this.mescroll = mescroll
    },
    refresh() {
      this.type = 0
      this.keyword = ''
      // this.cid = ''
      this.showMore = false
      this.scrollH = false
      this.mescrollUp.page.num = 1
      this.upCallback(this.mescrollUp.page, this.mescroll)
    },
    upCallback(page,mescroll){
        this.$http.post('mall/coupon/coupon-list',{model:'bbc'}, true, true).then((response) => {
            this.goodsList = true
            let arr = response.data.list
            if (page.num === 1) this.dataList = []
            // let p = page.num
            this.dataList = this.dataList.concat(arr)
            if (response.data && response.data.min_id) {
              this.mescrollUp.page.num = response.data.min_id
            } else {
              this.mescrollUp.page.num += 1
            }
            this.$nextTick(() => {
              mescroll.endSuccess(arr.length)
            })
          }).catch((e) => {
            mescroll.endErr()
          })
      }
  },
  mounted () {
    
  }
}
</script>

<style scoped lang="less">

</style>
