<template>
  <div id="home">
    <div class="olinav">
      <ul>
        <li @click="jumpTo('/olilist2')">
          <div>
            <img src="../../assets/img/oli/oliss3.png" alt="tu">
          </div>
          <p>线上油庄</p>
        </li>
        <li @click="jumpTo('/mall/underShop')">
          <div>
            <img src="../../assets/img/oli/oliss4.png" alt="tu">
          </div>
          <p>线下油站</p>
        </li>
        <li @click="jumpTo('/contact')">
          <div>
            <img src="../../assets/img/oli/oliss2.png" alt="tu">
          </div>
          <p>在线客服</p>
        </li>
        <li @click="jumpTo('/mall/Integralshop')">
          <div>
            <img src="../../assets/img/oli/oliss.png" alt="tu">
          </div>
          <p>积分商城</p>
        </li>
      </ul>
    </div>
    <!-- <div class="olilun">
      <div>今日热点</div>
      <div>
        <p>121212</p>
        <p>123312</p>
        <p>44444</p>
        <p>166666212</p>
        <p>166666212</p>
        <p>166666212</p>
        <p>166666212</p>
        <p>166666212</p>
        <p>166666212</p>
        <p>166666212</p>
      </div>
    </div> -->
    <div class="olilist">
      <div class="nav">
        <span>油券热卖</span>
        <span @click="jumpTo('/olilist')">查看更多</span>
      </div>
      <ul>
        <li v-for="item in list" :key="item.id" @click="tap(item)">
          <div>
            <img :src="item.oil_thumb" alt="">
          </div>
          <p>{{item.oil_name}}</p>
          <div>
            <span>￥{{item.oil_price}}</span>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import { Swiper, GroupTitle, SwiperItem, XButton, Divider } from 'vux'
export default {
  name: 'home',
  created() {
    this.getlist()
  },
  data() {
    return {
      list:[]
    }
  },
  components: {
    Swiper,
    SwiperItem,
    GroupTitle,
    XButton,
    Divider
  },
  methods :{
    getlist() { // 获取数据列表
      this.$http.post('/amoy/user/index-oil').then(res=>{
        if(res.code==0) {
          this.list=res.data
        }
      })
    },
    tap(item) { // 点击列表进入详情页
      // this.$router.push({path:"/oliinfo",query:{item : item}})
      this.jumpTo({
        path:"/oliinfo",
        query:{
          id:item.id,
          oil_desc:item.oil_desc,
          oil_name:item.oil_name,
          oil_price:item.oil_price,
          oil_thumb:item.oil_thumb
        }
      })
    }
  }
}
</script>

<style lang="less" >
@keyframes aa {
    from {
      transform: translateY(0)
    }
    to {
      transform: translateY(-3rem)
    }
  }
#home {
  background-color: #F7F7F7;
  .olilun {
    position: relative;
    width: 100%;
    height: 1rem;
    background-color: #fff;
    div:nth-of-type(1) {
      position: absolute;
      top: .3rem;
      left: .3rem;
      font-size: .33rem;
      font-weight: 700;
      color: red;
    }
    div:nth-of-type(2) {
      position: absolute;
      right: 0;
      width: 71%;
      height: 1rem;
      line-height: 1rem;
      font-size: .32rem;
      overflow: hidden;
      text-align: left;
      // background-color: pink;
      p {
        animation: aa 10s linear infinite;
        margin-left: .2rem;
      }
    }
    div:nth-of-type(1)::after {
      content:"";      
      position: absolute;
      left: 1.55rem;
      top: .1rem;
      width: 1px;
      height: .3rem;
      background:rgba(238,238,238,1);
    }
  }
}
.olinav {
  padding-top: .3rem;
  padding-bottom: .5rem;
  border-bottom: 0.5px solid #e7e7e7;
  background-color: #fff;
  ul {
    display: flex;
    justify-content: space-around;
    li {
      list-style: none;
      text-align: center;
      font-size: .25rem;
      font-weight:400;
      color:rgba(51,51,51,1);
      line-height: .5rem;
      div {
        width: .8rem;
        height: .8rem;
        margin: 0 auto;
        img {
          width: 100%;
        }
      }
    }
  }
}
.olilist {
  width: 93%;
  background-color: #fff;
  margin: .2rem auto 0;
  border-radius:10px; 
  padding-top:.3rem;
  box-sizing: border-box; 
  .nav {
    position: relative;
    display: flex;
    justify-content: space-between;
    font-size: .3rem;
    padding-right: .7rem;
    padding-bottom:.3rem; 
    border-bottom:0.5px solid #e7e7e7; 
    span:nth-of-type(1) {
      display: block;
      width: 1.5rem;
      height: .5rem;
      line-height: .5rem;
      text-align: center;
      color: #fff;
      font-size: .28rem;
      background:linear-gradient(24deg,rgba(255,81,90,1),rgba(255,150,14,1));
      border-radius: 0 5px 5px 5px;
    }
    span:nth-of-type(2) {
      font-size: .28rem;
      color: #666;
    }
    span:nth-of-type(2)::after {
      content:"";
      position: absolute;
      top: .05rem;
      right: .3rem;
      width: .3rem;
      height: .3rem;
      background: url("../../assets/img/oli/olilefss.png") no-repeat center center;
      background-size: 100% 100%; 
      border-radius:50%; 
    }
  }
  ul {
    overflow: hidden;
    display: flex;
    justify-content: space-between;
    padding:.35rem .2rem .35rem .3rem;
    flex-wrap:wrap;
    margin-bottom: .2rem;
    li {
      float: left;
      list-style: none;
      width: 48%;
      height: 4.4rem;
      margin-bottom: .2rem;
      margin-left: .12rem;
      div:nth-of-type(1) {
        width: 93%;
        height: 3rem;
        background-color: #fff;
        border-radius:5px;
        margin-bottom: .2rem;
        margin-right: -.2rem;
        overflow: hidden;
        img {
          width: 100%;
          height: 100%;
        } 
      }
      div:nth-of-type(2) {
        display: flex;
        justify-content: space-between;
        padding: 0 .3rem 0rem 0rem ;
        span:nth-of-type(1) {
          color:rgba(254,29,6,1);
          font-size: .3rem;
        }
        span:nth-of-type(2) {
          font-size: .25rem;
          color:#999999;
          text-decoration: line-through;
        }
      }
      p {
        font-size: .29rem;
        line-height: .37rem;
        width: 90%;
        margin-bottom: .15rem;
        text-align: left;
      }
    }
  }
}
</style>
