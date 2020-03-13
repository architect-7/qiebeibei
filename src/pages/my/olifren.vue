<template>
  <div class="hhhe">
    <!-- 邀请好友 -->
    <!--<div class="workeway">
      <span></span>
      <tab
        :line-width="1.5"
        custom-bar-width="60px"
        bar-active-color="#FF3E55"
      >
        <tab-item :selected="this.flag == true" @on-item-click="getjj">推广会员</tab-item>
        <tab-item :selected="this.ttt == true" @on-item-click="outjj">推广商家</tab-item>
      </tab>
    </div>-->
    <!-- 收支记录 -->
    <div class="forpp" v-if="flag" :style="{backgroundImage: 'url(' + imgs + ')' }">
     <div class="oone"></div>
       <div class="forr" v-if="imgs">
         <span @click="onCopy(list.downloadUrl)">复制链接</span>
         <span @click="savePicture()">保存海报</span>
       </div>
       <div id="forrr"></div>
    </div>
    
    <!-- 释放记录 -->
    <div class="forppp"  :style="{backgroundImage: 'url(' + imgs + ')' }">
    </div>
  </div>
</template>

<script>
import {Tab, TabItem} from 'vux'
import { log } from 'util';
export default {
  components :{
    Tab,
    TabItem
  },
  data() {
    return {
      data: this.$store.state.user.userInfo,
      info: this.$store.state.user.userInfo,
      list: {},
      alllist: [],
      page: 0,
      flag:true,
      ttt:false,
      imgs:'',
      acc:'./static/img/bgc.png'
    };
  },
  created() {
      this.erwe();
      
  },
  methods: {
    getjj() { //收支记录
      this.flag=true
      this.ttt=false
    },
    outjj() { //释放记录
      this.ttt=true
      this.flag=false
    },
    erwe() { // 获取分享链接
      this.$http.post('/amoy/user/invite',{},false,false).then(res=>{
        if(res.code==0) {
         this.list = res.data;
         this.imgs = config.url + config.bizId+'/amoy/user/invite?tpl=' + res.data.img[0] + '&uid=' + this.info.uid + '&invite_code=' + this.data.invite_code
         
         //this.beijing();
        }
      })
    },
    beijing() { // 获取分享图片
      this.$http.get('/amoy/user/invite',{
        biz_id:config.bizId,
        uid:this.data.uid,
        tpl:this.list.img[0],
        invite_code:this.data.invite_code
      },false,false).then(res=>{
        if(res.code==0) {

        }
      })
    },
    //保存图片
    savePicture(){
          var timestamp = new Date().getTime()
          api.download({
              url: this.imgs,
              savePath: 'fs://test'+timestamp+'.jpeg',
              report: true,
              cache: true,
              allowResume: true
          }, function(ret, err) {
              if(ret){
                  api.toast({
                      msg:'图片已保存到本地'
                  })
              }
              api.saveMediaToAlbum({
                  path: 'fs://test'+timestamp+'.jpeg'
              }, function(ret, err) {
                  
              });
          });
    },
    // 复制
    onCopy (orderSn) {
      if(!orderSn){
        this.$vux.toast.text("暂无分享连接");
        return;
      }
      let that = this
      let clipBoard = api.require('clipBoard')
      clipBoard.set({
        value: orderSn
      }, function (ret, err) {
        if (ret) {
          that.$vux.toast.text('分享链接复制成功')
        } else {
          that.$vux.toast.text('请重试')
        }
      })
    }
  }
};
</script>

<style scoped>
/* 头部 */
.hhhe {
  position: relative;
  overflow: hidden;
  height: 100%;
  width: 100%;
  box-sizing: border-box;
  background-color: #fff;
}
#viphead {
  position: fixed;
  top: 0;
  width: 100%;
  z-index: 99;
}
.heee {
  position: absolute;
  top: 0.27rem;
  left: 0.3rem;
  margin-bottom: .3rem;
}
/* 图片的图 */
.forpp,
.forppp
 {
  position: relative;
  width: 100%;
  height: 100%;
  
  background-color: #fff;
  /*background: url("../../assets/img/oli/beijign.png") no-repeat center center;*/
  background-repeat: no-repeat;
  background-position: center center;
  background-size: 100% 100%;
  font-size: .3rem;
  text-align: center;
}
  .workeway {
    position: relative;
    z-index: 999;
  }
  .workeway span {
    position: absolute;    
    z-index: 1;
    top: .21rem;
    left: 3.8rem;
    width: 1px;
    height: .4rem;
    background-color: #e7e7e7;
  }
  /* 二维码 */
  #maerwei {
    position: absolute;
    /* bottom: 1.5rem; */ 
    left: 50%;
    transform: translateX(-55%);
    width: 1.6rem;
    height: 1.6rem;
    background-color: blue;
  }
  #maerwei img {
    width: 100%;
    height: 100%;
  }
  #forrr {
    width: 100%;
    height: .7rem;
  }
  .oone {
    width: 100%;
    height: 6.5rem;
    /* background-color: red; */
  }
  .two {
    display: block;
    margin: auto;
    width: 2.48rem;
    height: .61rem;
    line-height: .61rem;
    border-radius:10px;
    color: #fff;
    font-weight: 700;
    border:  1px solid #fff;
  }
  .tree {
    color:rgba(255,255,255,1);;
    font-size: .28rem;
    line-height: .6rem;
    font-weight: 400;
  }
  .forr {
    display: flex;
    justify-content: space-around;
    /* margin-top: .3rem; */
  }
  .forr span {
    width: 1.86rem;
    height: .54rem;
    line-height: .54rem;
    border-radius: 20px;
    background-color: #01C2C7; 
    color: white;
    font-size: .25rem;
  }
  .forr span:nth-of-type(1) {
    margin-right:-1.7rem; 
  }
</style>