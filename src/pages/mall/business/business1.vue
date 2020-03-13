<template>
    <div class="business">
      <div v-for="(item,i) in list" :key="i">
          <div class="lists" @click="jumpTo({name:'mallHomeDetail',query:{title:item.id}})">
            <div class="list-head">
              <div class="list-logo" ><img :src="item.thumb || './static/img/under/111.png'" alt=""></div>
              <div class="list-center">
                <div>
                    {{item.goodsName}}
                </div>
                 <div style="height:.2rem;"></div>
                <div><span>销量 {{item.sales}}</span>
                    <span>库存 {{item.total}}</span>
                </div>
                <div style="height:.15rem;"></div>
                <div>价格<span style="color:red;"> {{item.spe_price}}</span></div>
              </div>
            </div>
          </div>
      </div>
    </div>
</template>

<script>
import './business1.less'
import { Rater } from 'vux'
export default {
  name: 'business',
  components: {Rater},
  data () {
    return {
      txtStyle: ''
    }
  },
  methods: {
    setTxt () {
      if (this.txtStyle === '') {
        this.txtStyle = 'transform:translateX(-2rem)'
      } else {
        this.txtStyle = ''
      }
    },
    // 删除
    deleteItem: function (index, i) {
      
      this.$http.post('/mall/users/shops-care-delete', {careId: index}).then(res => {
        this.$vux.toast.text('取消成功')
        this.$emit('f5')
        this.list.shopList.splice(i, 1)
      })
    },
    jump (id) {
      this.$emit('jump', id)
    }
    // goto(id){
    //   this.$router.push({
    //     name:'homeDetail',
    //     query:{
    //       title:id
    //     }
    //   })
    // }
  },
  props: ['showCancel', 'list'],
  filters: {
    
    str (val) {
      if (val.length <= 7) {
        return val
      } else if (val.length > 7) {
        return val.substring(0, 7) + '...'
      }
    }
  }
}
</script>

<style scoped>
.business{
  text-align: center;
}
</style>
