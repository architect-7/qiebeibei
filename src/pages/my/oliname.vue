<template>
    <div id="lionn">
        <!-- 实名 -->
        <div class="namme">
          <ul>
            <li>
              <span>姓名:</span>
              <input type="text" placeholder="请输入真实姓名" v-model="tname">
            </li>
            <li>
              <span>身份证:</span>
              <input type="text" placeholder="请输入身份证号码" v-model="idcard">
            </li>
          </ul>
        </div>
        <div class="btnn" @click="brnnn">确认</div>
    </div>
</template>

<script>
export default {
  created () {

  },
  data () {
    return {
      tname:"",
      idcard:"",
      data: this.$store.state.user.userInfo
    }
  },
  methods: {
    brnnn() {  //身份信息验证接口
      if(this.tname==""||this.idcard==""){
        this.$vux.toast.text('所有都要填写不能为空');return
      }else if(!(/^[1-9]{1}[0-9]{14}$|^[1-9]{1}[0-9]{16}([0-9]|[xX])$/.test(this.idcard))){
        this.$vux.toast.text('身份证号有误，请重新输入')
        return
      }else{
        this.$http.post('/mall/users/add-real-name',{
        real_name:this.tname,
        id_card:this.idcard
      },true).then(res=>{
        if(res.code==0) {
          this.data.identity_card=res.data
          this.goBack()
        }
      })
      }
    }
  }

}
</script>

<style scoped>
#lionn {
  width: 100%;
  height: 100%;
  background-color: #fff;
}
.namme {
    width: 100%;
    font-size: .33rem;
    box-sizing: border-box;
}
.namme li {
    list-style: none;
    height: .65rem;
    line-height: .65rem;
    margin-bottom:.03rem;
    padding: .3rem 0 0 .4rem;
    border-bottom:1px solid  #F7F7F7;
}
.namme li span{
  font-size: .3rem;
}
.namme li input {
    margin-left: .2rem;
    height: 100%;
    outline: none;
    border: none;
    font-size: .28rem;
}
.btnn {
    width: 60%;
    height: .8rem;
    border-radius:10px;
    text-align: center;
    line-height: .8rem;
    background:linear-gradient(90deg,#3FE7E5 0%,#10A9B0 100%);
    border-radius:5px;
    margin: 1rem auto;
    font-size: .32rem;
    color: #F7F7F7;
}
</style>
