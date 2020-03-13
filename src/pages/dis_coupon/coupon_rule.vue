<template>
    <div class="ptext" v-html="info">
     
    </div>
</template>

<script>

export default {
  name: 'coupon_rule',
  components: {},
  data () {
    return {
      data: '',
      info:''
    }
  },
  methods: {
    getConfig(){
       this.$http.post('/amoy/user/coupon', {}).then(res => {
          this.info = res.data.coupon
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
