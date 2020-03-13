<template>
    <div>
        <div class="yijian">
          <p>您的意见是我们前进的动力！</p>
          <p>
            <span id="acctiv">*</span>
            请在此输入您的意见...
          </p>
          <div>
              <textarea name="" id="" cols="30" rows="10" v-model="counts"></textarea>
          </div>
        </div>
        <div class="btnn" @click="send">提交</div>
    </div>
</template>

<script>
export default {
  data () {
    return {
      counts:""
    }
  },
  created () {
  },
  methods: {
    send() {  //添加反馈
    var that = this;
      if(this.counts=="") {
        this.$vux.toast.text('填写不能为空');return
      }else{
        this.$http.post('/mall/users/add-feed-back',{
        content:this.counts
      },true).then(res=>{
        if(res.code==0) {
          this.$vux.toast.text('谢谢您的建议');
          setTimeout(function(){
            that.jumpTo("/info");
          },1500)
          
        }
      })
      }
    }
  }
}
</script>

<style scoped>
.yijian {
    position: relative;
    font-size: .3rem;
    background-color: #fff;
    height: 5.5rem;
    padding: .3rem .3rem;
    box-sizing: border-box;
}
.yijian p {
    line-height: .6rem;
}
.yijian p:nth-of-type(1) {
  font-weight:500;
  color:rgba(51,51,51,1);
  font-size: .33rem;
}
.yijian p:nth-of-type(2) {
  margin-left: .3rem;
  font-weight:400;
  color:rgba(153,153,153,1);
  font-size: .28rem;
}
textarea {
    padding: .3rem;
    display: block;
    width: 98%;
    height: 3.4rem;
    margin: .2rem auto;
    resize: none;
    outline: none;
    border:1px solid rgba(238,238,238,1);
    border-radius:20px;
    box-sizing: border-box;
}
#acctiv {
    position: absolute;
    top: 1rem;
    left: .25rem;
    font-weight:bold;
    color:rgba(255,62,85,1);
    font-size: .6rem;
}
.btnn {
    width: 70%;
    height: 0.9rem;
    border-radius:10px;
    text-align: center;
    line-height: 0.9rem;
    background:linear-gradient(90deg,#0EA9AD 0%,#01C2C7 100%);
    border-radius:44px;
    margin: .6rem auto;
    font-size: .35rem;
    color: #F7F7F7;
}
</style>
