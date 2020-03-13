<template>
  <div id="my">
    <div class="poop">
      <div class="lef">
        <ul>
          <li @click="tap(1)" :class="{active:active==aa}">家用电器</li>
          <li @click="tap(2)" :class="{active:active==bb}">111</li>
          <li @click="tap(3)" :class="{active:active==cc}">111</li>
        </ul>
      </div>
      <div class="rig">
        <div>
          <span>家用电器</span>
          <span  @click="jumpTo('/olitype1')">全部</span>
        </div>
        <div class="shop">
          <ul>
            <li>
              <div class="tu" @click="jumpTo('/olitype1')">
                <img src="../../assets/img/oli/oliac2.png" alt="tu">
              </div>
              <p>电饭煲</p>
            </li>
            <li>
              <div class="tu">
                <img src="../../assets/img/oli/oliac2.png" alt="tu">
              </div>
              <p>电饭煲</p>
            </li>
            <li>
              <div class="tu">
                <img src="../../assets/img/oli/oliac2.png" alt="tu">
              </div>
              <p>电饭煲</p>
            </li>
            <li>
              <div class="tu">
                <img src="../../assets/img/oli/oliac2.png" alt="tu">
              </div>
              <p>电饭煲</p>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'my',
  methods:{
    tap(num) {
      // this.active = name;
      if(num==1) {
        this.aa=false
        this.bb=true
        this.cc=true
      }else if(num==2) {
        this.aa=true
        this.bb=false
        this.cc=true
      }
      else if(num==3) {
        this.aa=true
        this.bb=true
        this.cc=false
      }
    }
  },
  data() {
    return {
      active:'',
      aa:false,
      bb:true,
      cc:true
    }
  }
}
</script>

<style lang="less">
#my {
  width: 100%;
  height: 100%;
  overflow: hidden;
  background-color: #fff;
}
.poop {
  width: 100%;
  height: 100%;
  display: flex;
  .lef {
    width: 100px;
    height: 100%;
    background:rgba(242,242,242,1);
    overflow-y:scroll;
    ul {
      li {
        list-style: none;
        height: 1rem;
        line-height: 1rem;
        text-align: center;
        font-weight: 700;
      }
    }
  }
  .rig {
    flex-grow:1; 
    height: 100%;
    padding:.35rem; 
    div {
      position: relative;
      font-size: .28rem;
      display: flex;
      justify-content: space-between;
      padding-right:.4rem; 
      span:nth-of-type(1) {
        font-weight:bold;
        color:rgba(0,0,0,1);
      }
      span:nth-of-type(2) {
        font-weight:400;
        color:rgba(102,102,102,1);
        font-size: .25rem;
      }
        span:nth-of-type(2)::after {
        content:"";
        position: absolute;
        top: .06rem;
        right: .05rem;
        width: .26rem;
        height: .26rem;
        background: url("../../assets/img/oli/olilefs.png") no-repeat center center;
        background-size: 100% 100%; 
        border-radius:50%; 
      }
    }
    .shop {
      margin-top: .3rem;
      ul {
        display: flex;
        justify-content: space-between;
        flex-wrap: wrap;
        width: 100%;
        padding-left:.33rem; 
        li {
          list-style: none;
          width: 32%;
          height: 2.3rem;
          margin-bottom:.2rem; 
          margin-top: .1rem;
          text-align: center;
          .tu {
            width: 65%;
            height: 1.5rem;
            padding: 0;
            margin: 0 auto;
            img {
              width: 100%;
            }
          }
          p {
            line-height: .7rem;
            font-weight:400;
            font-size: .28rem;
          }
        }
      }
    }
  }
}
.active {
   background: #fff;
   color:#01C2C7;
 }
 ::-webkit-scrollbar {
/*隐藏滚轮*/
display: none;
}
</style>
