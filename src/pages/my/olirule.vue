<template>
    <div id="ruler" v-html="list">
       
    </div>
</template>

<script>
export default {
    data() {
        return {
            list:''
        }
    },
    created() {
        this.gethos()
    },
    methods :{
        gethos() {
            this.$http.post("/amoy/user/article-list",{cid:"16"}).then(res=>{
                if(res.code==0) {
                    this.list=res.data[0].content
                    // alert(JSON.stringify(res.data[0].content))
                }
            })
        }
    }

}
</script>

<style>
  #ruler {
      padding: .3rem;
      box-sizing: border-box;
      width: 100%;
      height: 100%;
      background-color: #fff;
  }
</style>
