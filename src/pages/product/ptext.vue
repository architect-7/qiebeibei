<template>
    <div class="ptext" v-html="info.info">
     
    </div>
</template>

<script>

export default {
  name: 'ptext',
  components: {},
  data () {
    return {
      data: '',
      info:''
    }
  },
  methods: {
    getConfig(){
       this.$http.post('/mall/second-goods/config', {}).then(res => {
          this.info = res.data.addGoods
       })
    },
  },
  mounted () {
    this.getConfig()
  }
}
</script>

<style scoped lang="less">
.ptext{
  width:100%;
  box-sizing: border-box;
  padding:0.3rem;
  color:gray;
  font-size:0.25rem;
}
</style>
