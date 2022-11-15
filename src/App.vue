<script setup lang="ts">
import Loading from './components/Loading.vue'

import { ref, onMounted, onUnmounted } from 'vue';

const videoSrc = ref('videos/video.mp4');
const loading = ref(false);
const videoVisible = ref(true);
const exampleSrc = ref('')
let scanCode = '';

const keyUpHandler = (e: KeyboardEvent):void => {
  if (e.key !== "Shift" && e.key !== "Enter" && e.key !== "Alt") {
    scanCode += e.key;
  } else if(e.key === "Enter") {
    console.log(scanCode)
    scanCode = '';
    exampleSrc.value = '/example/white.png'
    loading.value = true;
    videoVisible.value = false;
  }

}

onMounted(() => {
  document.addEventListener('keydown', keyUpHandler)
});

onUnmounted(() => {
  document.removeEventListener('keydown', keyUpHandler)
});

</script>

<template>
  <video v-if="videoVisible" :src="videoSrc" autoplay loop></video>
  <div v-else class="m-main">
    <Loading v-show="loading" :loading="loading" :wait="2000" @change="loading = false" />
    <div v-show="!loading" class="m-example" style="color: #fff">
      <img class="m-bg" src="./assets/bg.png" alt="">
      <div class="m-pic-box">
        <div class="m-pic">
          <img :src="exampleSrc" alt="">
        </div>
        <p style="margin-top: 28px">PX（Pixel）Block</p>
        <p>DIAMOND LOVE #BLACK</p>
      </div>
      <div class="m-num-info">
        <h3>PX Block DIAMOND LOVE 钻石爱心像素系列图腾</h3>
        <p>该系列是PXBlock重要的IP视觉符号，以DIAMOND LOVE 钻石爱心像素，加持双爱心眼睛意旨特别温暖情绪，心动瞬间，爱意满满</p>
        <div class="m-line"></div>
        <div class="m-tips">
          PX(Pixel) Block 团队来自于概念艺术家、时尚潮流创意先锋、奢侈品牌装置艺术合作创作单位、媒体意见领袖等。
        </div>
        <div class="m-tips">
          PXBlock是以像素Pixel为核心表现的数字艺术内容创作单位,专注于数字艺术与公共、时尚艺术领域的创作，探索全新的数字艺术表现方式。
        </div>
      </div>

      <div class="m-info">
        <div class="m-content">
          <div class="m-size">
            <img style="display: block" src="./assets/chi_ma.png" alt="">
            <div class="m-btn">S</div>
            <div class="m-btn">M</div>
            <div class="m-btn active">L</div>
          </div>
          <div class="m-tu-an">
            <img style="display: block" src="./assets/tu_an.png" alt="" >
            <div>
              <div class="m-img active">
                <img src="/example/heart1.png" alt="">
              </div>
              <div class="m-img">
                <img src="/example/heart2.png" alt="">
              </div>
              <div class="m-img m-other">
                <span>其他系列</span>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="m-footer">
        <div class="m-line">
          <div class="m-left">
            <h3>NFT发行量-399</h3>
          </div>
          <div class="m-right"><span>立即下单</span></div>
        </div>
        <div style="text-algin: right; margin-top: 48px">
          <img src="./assets/bg3.png" alt="">
        </div>
      </div>

    </div>
  </div>
</template>

<style>
html,
body,
#app,
h3,
p,
div {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

::-webkit-scrollbar {
  display: none;
}

#app {
  width: 1920px;
  height: 100vh;
  background-color: #000;
}
#app video {
  width: 1920px;
  height: auto;
  vertical-align: middle;
}
.m-main {
  width: 100%;
  height: 100%;
}
.m-example {
  position: relative;
}
.m-example .m-bg {
  position: absolute;
  -webkit-user-drag: none;
}
.m-example .m-pic-box {
  position: absolute;
  top: 189px;
  left: 70px;
  width: 630px;
}
.m-example .m-pic {
  width: 100%;
  height: 668px;
  border-radius: 20.5px;
  background: linear-gradient(to bottom, #474747, #181818);
}
.m-example .m-pic-box p {
  height: 33px;
  font-size: 24px;
  font-family: AlibabaPuHuiTi-Bold, AlibabaPuHuiTi;
  font-weight: bold;
  color: #FFFFFF;
  line-height: 33px;
  -webkit-background-clip: text;
  text-align: center;
}

.m-example .m-num-info {
  position: absolute;
  top: 79px;
  left: 861px;
  width: 960px;
  height: 303px;
  padding: 70px 30px 30px 30px;
  background: url("./assets/border.png");
}
.m-example .m-num-info h3 {
  margin-bottom: 20px;
  height: 26px;
  font-size: 20px;
  font-family: AlibabaPuHuiTi-Bold, AlibabaPuHuiTi;
  font-weight: bold;
  color: #FFFFFF;
  line-height: 26px;
  -webkit-background-clip: text;
}
.m-example .m-num-info p{
  margin-bottom: 14px;
  width: 890px;
  font-size: 18px;
  height: 78px;
  font-family: AlibabaPuHuiTi-Regular, AlibabaPuHuiTi;
  font-weight: 400;
  color: rgba(255,255,255,0.8);
  line-height: 26px;
  -webkit-background-clip: text;
  }
.m-example .m-num-info h3::after {
  content: "";
  display: inline-block;
  margin-left: 20px;
  width: 16px;
  height: 16px;
  background-color: #FFFFFF;
}
.m-example .m-num-info .m-line {
  margin-bottom: 20px;
  width: 395px;
  height: 2px;
  background-color: #fff;
}
.m-example .m-num-info .m-tips {
  margin-top: 10px;
  width: 920px;
  font-size: 15px;
  font-family: AlibabaPuHuiTi-Regular, AlibabaPuHuiTi;
  font-weight: 400;
  color: rgba(255,255,255,0.6);
  line-height: 15px;
  -webkit-background-clip: text;
}

.m-example .m-info {
  position: absolute;
  padding: 69px 20px 20px 20px;
  left: 861px;
  top: 479px;
  width: 962px;
  height: 330px;
  background: url("./assets/border2.png");
}
.m-example .m-info .m-content {
  display: flex;

}
.m-example .m-info .m-content .m-size {
  width: 302px;
}
.m-size .m-btn {
  margin-top: 10px;
  width: 130px;
  height: 50px;
  line-height: 50px;
  text-align: center;
  color: #fff;
  border: 1px solid rgba(255, 255, 255, .3);
  cursor: pointer;
}
.m-size .active {
  background-color: #474747;
}
.m-example .m-info .m-content .m-tu-an > div{
  display: flex;
  margin-top: 10px;
}
.m-tu-an .m-img {
  width: 170px;
  height: 170px;
  cursor: pointer;
  border: 1px dashed rgba(255, 255, 255, .3);
}
.m-img + .m-img {
  margin-left: 40px;
}
.m-tu-an .active {
  background-color: #474747;
}

.m-tu-an .m-other {
  display: flex;
  align-items: center;
  justify-content: center;
  border-style: solid;
}
.m-tu-an .m-other span{
  height: 29px;
  font-size: 21px;
  font-family: AlibabaPuHuiTi-Regular, AlibabaPuHuiTi;
  font-weight: 400;
  color: rgba(255,255,255,0.8);
  line-height: 29px;
  letter-spacing: 2px;
  -webkit-background-clip: text;
}
.m-tu-an .m-other span::after {
  content: "";
  display: block;
  width: 100%;
  border-top: 1px solid #fff;
}
.m-footer {
  position: absolute;
  left: 861px;
  top: 902px;
}
.m-footer .m-line {
  display: flex;
  width: 962px;
  justify-content: space-between;
}
.m-footer .m-left h3 {
  height: 40px;
  font-size: 18px;
  font-family: Source Han Sans CN-Bold, Source Han Sans CN;
  font-weight: bold;
  color: #FDFDFD;
  line-height: 39px;
  letter-spacing: 3px;
  -webkit-background-clip: text;
}
.m-footer .m-left h3::after {
  margin-top: 10px;
  display: block;
  content: "";
  width: 210px;
  height: 10px;
  background-color: #fff;
}
.m-footer .m-line .m-right {
  width: 160px;
  height: 60px;
  line-height: 60px;
  text-align: center;
  background-color: #fff;
  border-radius: 8px;
  cursor: pointer;
}
.m-footer .m-line .m-right span {
  font-size: 30px;
  font-family: AlibabaPuHuiTi-Bold, AlibabaPuHuiTi;
  font-weight: bold;
  color: #000000;
  user-select: none;
  -webkit-background-clip: text;
}
</style>
