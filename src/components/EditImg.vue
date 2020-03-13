<template>
    <div>
      <!--<div class="kill-t">
          <div>限时秒杀</div>
          <div>{{count}}</div>
      </div>-->
      <div class="EditImg" :style="{backgroundColor: data.bgColor}">
        <div v-for="(item, index) of data.list" :key="index" class="oneImg"   @click="handDetail(item.goodsId)">
          <img style="margin-bottom:0.1rem;"  :src="item.goodsImg" alt="">
        </div>
      </div>
    </div>
</template>

<script>
export default {
  name: 'EditImg',
  components: {},
  props: ['data'],
  data () {
    return {
      second:'',
      timer:'',
      count:'00:00:00',
      endTime:'',
    }
  },
  mounted(){
    //this.endTime = (parseInt(this.data.endTime*1000)-parseInt((new Date()).getTime()))/1000
    /*this.endTime = this.data.endTime
    var that = this;
    if(this.endTime){
      that.timer = setInterval(function(){
        that.transfer_time()
     },1000)
    }*/
  },
  methods: {
     handDetail (id) {
      this.$router.push({
        name: 'mallHomeDetail',
        query: {
          title: id
        }
      })
    },
  },
  destroyed(){
    clearInterval(this.timer)
  }
}
</script>

<style scoped lang="less">
.kill-t{
    display:flex;
    justify-content: space-between;
    align-items:center;
    padding:0.3rem;
    box-sizing: border-box;
    div:first-child{
      font-size:0.3rem;
      color:#333333;
      font-weight:bold
    }
    div:last-child{
      width:1.2rem;
      height:0.4rem;
      line-height: 0.4rem;
      text-align: center;
      font-size:0.25rem;
      color:#FF1D1C;
      border-radius:5px;
      border:1px solid #FF1D1C;
    }
}
.EditImg{
  background:white;
  margin-top: -1px;
  overflow: hidden;
  display:flex;
  width:100%;
  padding:0.3rem;
  box-sizing: border-box;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  padding-top:0;
  .oneImg{
    overflow: hidden;
    img{
      width:3.4rem;
      float: left;
    }
  }
}
</style>
