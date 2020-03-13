<template>
    <div class="productList">
      <x-header :left-options="{showBack: false}" class="category-head" >
        <a @click="goBack()" slot="left" class="backBox"><i class="iconfont icon-back"></i></a>
        <a  class="head-right">{{title}}</a>
      </x-header>
      <div class="productList-nav">
       
      </div>
      <List :list="list" style="margin-top: 1.9rem;height: calc(100% - 105px);overflow-y: auto"></List>
    </div>
</template>

<script>
import {XHeader} from 'vux'
import List from '../../../components/list'
import * as apiHttp from '../../../api/index'
export default {
  name: 'productList',
  components: {
    XHeader,
    List
  },
  data () {
    return {
      price: true,
      sales: true,
      select: 1,
      title: '',
      list: '',
      title:this.$route.query.title
    }
  },
  methods: {
    getList () {
      if (this.$route.query.id) {
        this.$http.post('/mall/index/goods-list', {
          cid: this.$route.query.id,
          keyword: this.$route.query.keyword
        }).then(res => {
          if (res.code === 0) {
            this.list = res.data.list
          } else {
            this.$vux.toast.text(res.msg)
          }
        })
      } else if (this.$route.query.spid) {
        this.$http.post('/mall/index/goods-list', {
          spid: this.$route.query.spid,
          keyword: this.$route.query.keyword
        }).then(res => {
          if (res.code === 0) {
            this.list = res.data.list
          } else {
            this.$vux.toast.text(res.msg)
          }
        })
      } else {
        this.$http.post('/mall/index/goods-list', {
          // spid: this.$route.query.spid,
          keyword: this.$route.query.keyword
        }).then(res => {
          if (res.code === 0) {
            this.list = res.data.list
          } else {
            this.$vux.toast.text(res.msg)
          }
        })
      }
    },
    setTitle () {
      if (this.$route.query.keyword) {
        this.title = this.$route.query.keyword.substring(0, 4) + '...'
      } else if (this.$route.query.title) {
        this.title = this.$route.query.title
      } else {
        this.title = '商品列表'
      }
    }
  },
  mounted () {
    // 删除
    
  }
}
</script>

<style scoped>

</style>
