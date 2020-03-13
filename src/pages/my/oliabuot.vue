<template>
    <div class="about">
        <div id="yingt">
            <img src="" alt="图">
            <span>企鹅日记</span>
        </div>
        <div class="btnmm">
            <p>当前版本版本：V 1.0.0</p>
        </div>
    </div>
</template>

<script>
export default {

}
</script>

<style scoped>
#heeade {
    position: relative;
    width: 100%;
    height: 100%; 
}
.heee {
    position: absolute;
    top: .27rem;
    left: .3rem;
}
/* 顶部 */
.about {
    position: relative;
    height: 100%;
    width: 100%;
    background-color: #fff;
    overflow: hidden;
}
/* 内容 */
.linee {
    position: absolute;
    height: 1px;
    width: 100%;
    top: 1rem;
    background-color: #F7F7F7;
}
#yingt {
    position: absolute;
    top: 1rem;
    left: 50%;
    transform: translateX(-50%);
    z-index: 99;
    width: 1.6rem;
    height: 1.6rem;
    border-radius:5px;
    margin: 1.5rem auto; 
}
#yingt img {
    width: 100%;
}
#yingt span {
    width: 200%;
    position: absolute;
    top: 2rem;
    left: -.1rem;
    font-size: .35rem;
    font-weight:500;
    color:rgba(51,51,51,1);
}
.btnmm {
    width: 100%;
    position: absolute;
    bottom: .8rem;
    font-size: .33rem;
    text-align: center;
    line-height: .6rem;
}
.btnmm p:nth-of-type(2) {
    font-size: .3rem;
    font-weight:400;
    color:rgba(153,153,153,1);
}
</style>
