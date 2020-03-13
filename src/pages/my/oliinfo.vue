<template>
    <div id="oliso">
      <div class="lisy">
          <div class="tutu">
            <img :src="oil_thumb" alt="">
          </div>
          <p v-html="oil_name" class="nname"></p>
          <div class="ziwzi">
            <span>￥{{oil_price}}</span>
          </div>
          <div class="emp"></div>
          <p id="xianga">商品详情</p>
          <div id="xiang" v-html="oil_desc" ></div>
      </div>
      <div class="bot-btn">
        <div id="bttno" @click="tap({prc:oil_price,id:oliid})">点击购买</div>
      </div>
      
    </div>
</template>

<script>
export default {
  data() {
    return {
      
    }
  },
  methods:{
    tap (pric) { // 产生 订单
      this.$http.post('/amoy/user/oil-order',{
        id: this.oliid
      }).then(res=>{
        if(res.code==0) {
          let som = {
            ...pric
          }
          som.dan=res.data
          this.$router.push({path:"/olibuy", query:{pric : som}})
        }
       })
    }
  },
  computed:{ // 接收的参数，解决返回数据丢失问题
    oil_thumb() {
      return this.$route.query.oil_thumb
    },
    oil_price() {
      return this.$route.query.oil_price
    },
    oil_name() {
      return this.$route.query.oil_name
    },
    oil_desc() {
      return this.$route.query.oil_desc
    },
    oliid() {
      return this.$route.query.id
    }
  },
  created() {
  }

}
</script>

<style lang="less">
#oliso {
  width: 100%;
  height: 100%;
  background-color: #fff;
}
.lisy {
  .tutu {
      width: 100%;
      height: 4rem;
      img {
        width: 100%;
        height: 100%;
      }
  }
  .nname {
    margin-left: .3rem;
  }
  p {
    margin:.1rem .15rem;
    font-size: .3rem;
  }
  #xianga {
    position: relative;
    margin-left: .5rem;
  }
  #xianga::after {
    content: "";
    position: absolute;
    top: .1rem;
    left: -.2rem;
    width: .05rem;
    height: .3rem;
    background-color: red;
  }
  .ziwzi {
      padding:0 .2rem;
    span:nth-of-type(1) {
      color:rgba(254,29,6,1);
      font-size: .33rem;
      margin-right: .1rem;
    }
    span:nth-of-type(2) {
      font-size: .25rem;
      color: #666;
      text-decoration-line: line-through;
    }
  }
  .emp {
      margin-top: .2rem;
      width: 100%;
      height: .15rem;
      background-color: #F7F7F7;
  }
  #xiang {
      width: 100%;
      padding: .2rem;
      margin-bottom:50px;
      box-sizing: border-box;
  }
}
.bot-btn{
  width: 100%;
  height:1rem;
  line-height: 1rem;
  position: fixed;
  bottom:0;
  left:0;
  text-align: center;
  background:white;
  padding-top:0.15rem;
}
#bttno {
   
    width: 60%;
    height: .8rem;
    line-height: .8rem;
    background-color: #FD374C;
    margin:0 auto;
    text-align: center;
    border-radius:5px; 
    color: #fff;
    font-size: .3rem;
}
</style>