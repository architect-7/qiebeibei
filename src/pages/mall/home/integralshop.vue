<template>
    <div class="productList">
      <x-header :left-options="{showBack: false}" class="category-head" >
        <a @click="goBack()" slot="left" class="backBox"><i class="iconfont icon-back"></i></a>
        <a  class="head-right">积分商城</a>
        </x-header>
      <div class="indeify">您的积分为：<span style="color:red">{{userInfo.credit1}}</span> 积分 </div>
      <div class="productList-nav">
        <div :class="{'focus':select===1}" @click="selectItem(1)">全部</div>
        <div :class="{'focus':select===2}" @click="selectItem(2)">最新</div>
        <div :class="{'focus':select===3}" @click="selectItem(3)">
          价格
          <img src="../../../assets/mall/img/business/logo/jiantou.jpg" alt="" v-if="price">
          <img src="../../../assets/mall/img/business/logo/jiantou1.jpg" alt="" v-if="!price">
        </div>
        <div :class="{'focus':select===4}" @click="selectItem(4)">
          销量
          <img src="../../../assets/mall/img/business/logo/jiantou.jpg" alt="" v-if="sales">
          <img src="../../../assets/mall/img/business/logo/jiantou1.jpg" alt="" v-if="!sales">
        </div>
      </div>
      <T-List :list="list" style="margin-top: 1.9rem;height: calc(100% - 105px);overflow-y: auto"></T-List>
    </div>
</template>

<script>
import {XHeader} from 'vux'
import TList from '../../../components/tlist'
export default {
  name: 'integralshop',
  components: {
    XHeader,
    TList
  },
  data () {
    return {
      price: true,
      sales: true,
      select: 1,
      title: '',
      list: '',
      userInfo:{}
    }
  },
  methods: {
    getUserInfo(){
      this.$http.post('/amoy/user/user-info').then(res => {
            this.userInfo = res.data
       })
    },
    selectItem (type) {
      this.select = type
      if (type === 3) {
        this.price = !this.price
      } else if (type === 4) {
        this.sales = !this.sales
      }
      let obj = {}
      if (type === 1) {
        obj = {
          isRecommend: 1,
          cid: this.$route.query.id,
          keyword: this.$route.query.keyword
        }
      } else if (type === 2) {
        obj = {
          time: 1,
          cid: this.$route.query.id,
          keyword: this.$route.query.keyword
        }
      } else if (type === 3) {
        if (this.price === true) {
          obj = {
            price: 1,
            desc: 0,
            cid: this.$route.query.id,
            keyword: this.$route.query.keyword
          }
        } else if (this.price === false) {
          obj = {
            price: 1,
            desc: 1,
            cid: this.$route.query.id,
            keyword: this.$route.query.keyword
          }
        }
      } else if (type === 4) {
        if (this.sales === true) {
          obj = {
            sales: 1,
            desc: 0,
            cid: this.$route.query.id,
            keyword: this.$route.query.keyword
          }
        } else if (this.sales === false) {
          obj = {
            sales: 1,
            desc: 1,
            cid: this.$route.query.id,
            keyword: this.$route.query.keyword
          }
        }
      }
      this.$http.post('/mall/index/integral-goods', obj).then(res => {
        if (res.code === 0) {
          this.list = res.data.list
        } else {
          this.$vux.toast.text(res.msg)
        }
      })
    },
    getList () {
      if (this.$route.query.id) {
        this.$http.post('/mall/index/integral-goods', {
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
        this.$http.post('/mall/index/integral-goods', {
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
        this.$http.post('/mall/index/integral-goods', {
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
    
  },
  mounted () {
    // 删除
    this.getList()
    this.getUserInfo()
  }
}
</script>

<style scoped>
   .integralshop{
        width:100vw;
        font-size:4vw;
        color:rgb(51,51,51)
    }
    .integralshop>.ticket{
        padding:2vw;
        border-bottom:5px solid rgb(240,240,240)
    }
    .integralshop>.ticket>.cred{
      color:red
    }
    .ticket-list>.tab{
        width:96vw;
        padding:2vw;
        display:flex;
        justify-content: space-between;
        align-items:center
    }
    .productList-nav{
      position:absolute;
      top:80px;
    }
    .indeify{
      font-size:3vw;
      position:absolute;
      top:45px;
      width:96vw;
      background:rgb(235,235,235);
      padding:2vw;
    }
</style>
