<template>
  <div class="applyRefu">
      <div class="apply-box">
          <div class="apply-1">
              <textarea v-model="content"></textarea>
          </div>
          <div class="apply-2" @click="uploadImg()"><img align="absmiddle" src="../../../assets/mall/img/home/scpz.png" alt=""> 请上传凭证,仅限一张</div>
          <div class="apply-3" v-if="img">
              <div class="upload-img" :style="{ 'background': 'url(' + img + ') no-repeat center center', 'background-size': '2rem 2.2rem'}">
                  <img @click="delImg()" class="pro-close" src="../../../assets/img/product/guanbi.png" alt="">
              </div>
          </div>
      </div>
      <div class="apply-submit" @click="confirmSubmit()">确认提交</div>
 </div>
</template>

<script>
import * as apiHttp from '../../../api/index'
import {formatDate} from '../../../plugin/data'
export default {
  name: 'applyRefu',
  data () {
    return {
      content:'请输入退款原因(1-200字)',
      img:''
    }
  },
  mounted() {
    
  },
  methods: {
    confirmSubmit(){
        apiHttp.orderApplyReturn(this.$route.query.id,this.content,this.img).then(response => {
            if (response.code === 0) {
               this.$vux.toast.text(res.msg);
            }else{
                this.$vux.toast.text(res.msg);
            }
        })
    },
    delImg(){
        this.img = ''
    },
    uploadImg(){
        var that = this;
        api.getPicture({
            sourceType: 'album',
            encodingType: 'jpg',
            mediaValue: 'pic',
            destinationType: 'base64',
            allowEdit: true,
            quality: 100,
            targetWidth: 750,
            targetHeight: 750,
            saveToPhotoAlbum: false
         }, function(ret, err) {
            if (ret) {
                if(ret.base64Data){
                  that.img = ret.base64Data
                }
             }
         })
    }
  }
}
</script>

<style scoped lang="less">
.applyRefu{
    width:100%;
    font-size:0.3rem;
    color:#666666;
    .apply-box{
        width:100%;
        box-sizing: border-box;
        height:6rem;
        padding:0.3rem;
        background:white;
        .apply-3{
            margin-top:0.2rem;
            .upload-img{
                position:relative;
                width:1.6rem;
                height:2rem;
                border-radius:0.1rem;
                img{
                    position:absolute;
                    width:0.3rem;
                    height:0.3rem;
                    top:0.1rem;
                    right:0.1rem;
                }
            }
        }
        .apply-2{
            img{
                width:0.8rem;
                height:0.8rem;
            }
        }
        textarea{
            width:100%;
            height:2rem;
            border:0;
            outline: none;
            color:#666666;
            font-size:0.3rem;
        }
    }
    .apply-submit{
        width:6rem;
        height:0.8rem;
        text-align: center;
        line-height: 0.8rem;
        margin:0.5rem auto;
        border-radius:3px;
        background:#01C2C7;
        color:white;
    }
}
</style>
