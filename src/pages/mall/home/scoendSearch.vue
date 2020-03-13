<template>
  <div class="search_main">
      <div class="header">
        <div class="home_seach_left" @click="goBack()">
          <a  slot="left" class="backBox"><i class="iconfont icon-back"></i></a>
        </div>
        <div class="home_seach_input">
          <div class="input_box">
            <img src="../../../assets/mall/img/home/seach.png" alt="">
            <input type="text" placeholder="请输入关键字" v-model="text">
          </div>
        </div>
        <div class="home_seach_right"  @click="jumpTo({name:'mallProductList',query:{keyword:text,model:model}})">
          搜索
        </div>
      </div>
  </div>
</template>

<script>
import { Confirm, TransferDomDirective as TransferDom } from 'vux'
import * as apiHttp from '../../../api/index'
export default {
  name: 'scoendSearch',
  directives: {
    TransferDom
  },
  data () {
    return {
      show: false,
      listAll: [],
      listNew: [],
      text: '',
      model:this.$route.query.model
    }
  },
  components: {
    Confirm
  },
  mounted: function () {
    apiHttp.search(this.$route.query.type).then(response => {
      if (response.code === 0) {
        this.listNew = response.data.userSearchList
        this.listAll = response.data.searchList
      }
    })
  },
  methods: {
    
    
  }
}
</script>

<style scoped>

</style>
