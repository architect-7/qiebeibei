<template>
    <div class="suc">
      <div>
        <img src="../../assets/img/oli/suc.png" alt="">
      </div>
      <p>充值成功</p>
      <span>充值金额：{{num}}</span>
    </div>
</template>

<script>
export default {
    data() {
        return {
            num:'￥3200'
        }
    }
}
</script>

<style scoped lang="less">
  .suc {
      div {
          margin: 1.5rem auto .3rem ;
          width: 1.1rem;
          height: 1.1rem;
        //   background-color: red;
          img {
              width: 100%;
              height: 100%;
          }
      }
      p {
        font-size:17px;
        font-family:PingFang-SC-Medium;
        font-weight:500;
        color:rgba(0,0,0,1);
        line-height:25px;
        text-align: center;
      }
      span {
        display: block;
        font-size:12px;
        font-family:PingFang-SC-Regular;
        font-weight:400;
        color:rgba(51,51,51,1);
        line-height:25px;
        text-align: center;
        line-height: .6rem;
      }
  }
</style>
