<script setup lang="ts">
import Loading from './components/Loading.vue'

import { ref, onMounted, onUnmounted, computed, watch } from 'vue';
const log = require('electron-log');

const videoSrc = ref('videos/video.mp4');
const loading = ref(false);
const videoVisible = ref(true);
const pageNum = ref(0);
const selectedImgIndex = ref(0);
const selectedSize = ref('S');
const white = ref(true)
let scanCode = '';

const username = ref('小明');
const phoneNum = ref ('15000000000');

const errorMsgVisible = ref(false);
const canScan = ref(true)

const config = [
  { 
    id: '8173641A9C4504',
    type: 'white',
    size: 'S'
  },
  {
    id: '8173641A9C4104',
    type: 'black',
    size: 'M'
  }
]

const keyUpHandler = (e: KeyboardEvent):void => {
  if (!canScan.value) return
  if (e.key !== "Shift" && e.key !== "Enter" && e.key !== "Alt") {
    scanCode += e.key;
  } else if(e.key === "Enter") {
    const keyItem = config.find(item => item.id === scanCode)
    scanCode = '';
    if (!keyItem) return
    white.value = keyItem?.type === 'white' ? true : false;
    if (white.value) {
      selectedImgIndex.value = 0
    } else {
      selectedImgIndex.value = 1
    }
    selectedSize.value = keyItem.size;
    pageNum.value = 0
    loading.value = true;
    videoVisible.value = false;
  }

}

const exampleSrc = computed(() => {
  return white.value ? 'example/white.png' : 'example/black.png'
});

watch(selectedImgIndex, (val) => {
  if (val === 0 || val === 2) {
    white.value = true
  } else {
    white.value = false
  }
})


const chooseImg = (e: MouseEvent) => {
  const img = e.target;
  if (img) {
    const index = parseInt((img as HTMLElement).getAttribute('id') as string)
    if (index >= 0) selectedImgIndex.value = index
  }
}

const chooseSize = (s: string) => {
  selectedSize.value = s
}

const chooseImgIndex = (i: number) => {
  selectedImgIndex.value = i
}

const toImgPage = (i: number) => {
  selectedImgIndex.value = i
  pageNum.value = 1
}

const toVideo = () => {
  videoVisible.value = true
}

const clearScanCode = () => {
  scanCode = ''
}

const submitTrade = () => {
 if (!checkPhone()) return
  pageNum.value = 3
  log.info(JSON.stringify({ name: username.value, phone: phoneNum.value }));
}

const checkPhone = (): boolean => {
  canScan.value = true
  const regx = /^1[3456789]\d{9}$/
  if (!regx.test(phoneNum.value)) {
    errorMsgVisible.value = true
    // console.log('手机号不合法')
    return false
  }
  errorMsgVisible.value = false
  return true
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
  <div v-else class="m-main" @click="clearScanCode">
    <div class="m-video-btn" @click="toVideo"></div>
    <Loading v-show="loading" :loading="loading" :wait="2000" @change="loading = false" />
    <div v-show="!loading && pageNum === 0" class="m-example">
      <img class="m-bg" src="./assets/bg.png" alt="">
      <div class="m-pic-box">
        <div class="m-pic">
          <img :src="exampleSrc" alt="">
        </div>
        <p style="margin-top: 28px">PX（Pixel）Block</p>
        <p>DIAMOND LOVE <em>#{{ white ? 'WHITE' : 'BLACK' }}</em></p>
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

      <div class="m-brand">
        <img src="./assets/logo.png" alt="">
        <p>UNTILWERICH品牌成立于2021年，隶属于杭州满座电子商务有限公司旗下。品牌设计主要以美式街头风格为主，从说唱、滑板、经典、艺术等多个领域吸取创作灵感，通过服装、配饰、饰品等多个类目上的设计传达品牌思想以及态度。</p>
      </div>

      <div class="m-info">
        <div class="m-content">
          <div class="m-size">
            <img style="display: block" src="./assets/chi_ma.png" alt="">
            <div id="S" class="m-btn" :class="selectedSize === 'S' ? 'active' : ''" @click="chooseSize('S')">S</div>
            <div id="M" class="m-btn" :class="selectedSize === 'M' ? 'active' : ''" @click="chooseSize('M')">M</div>
            <div id="L" class="m-btn" :class="selectedSize === 'L' ? 'active' : ''" @click="chooseSize('L')">L</div>
          </div>
          <div class="m-tu-an">
            <img style="display: block" src="./assets/tu_an.png" alt="" >
            <div>
              <div class="m-img" :class="selectedImgIndex === 0 ? 'active' : ''" @click="chooseImgIndex(0)">
                <img src="/example/heart1.png" alt="">
              </div>
              <div class="m-img" :class="selectedImgIndex === 1 ? 'active' : ''" @click="chooseImgIndex(1)">
                <img src="/example/heart2.png" alt="">
              </div>
              <div class="m-img m-other" :class="selectedImgIndex > 1 ? 'active' : ''" @click="toImgPage(2)">
                <span>其他系列</span>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="m-footer">
        <div class="m-line">
          <div class="m-left">
            <h3></h3>
          </div>
          <div class="m-price">
            <span>合计</span>
            <strong>299元</strong>
          </div>
          <div class="m-right" @click="pageNum = 2">
            <span>立即下单</span>
          </div>
        </div>
        <div style="text-algin: right; margin-top: 48px">
          <img src="./assets/bg3.png" alt="">
        </div>
      </div>

    </div>
    <div v-show="!loading && pageNum === 1" class="m-example m-page-1">
      <img class="m-bg" src="./assets/bg.png" alt="">
      <ul class="m-content" @click="chooseImg">
        <li>
          <div :class="selectedImgIndex === 2 ? 'active': ''"><img id="2" src="/example/white_l.png" alt=""></div>
          <div :class="selectedImgIndex === 3 ? 'active': ''"><img id="3" src="/example/black_l.png" alt=""></div>
          <div class="disabled" :class="selectedImgIndex === 4 ? 'active': ''"><img id="4" src="/example/disabled_1.png" alt=""></div>
          <div class="disabled" :class="selectedImgIndex === 5 ? 'active': ''"><img id="5" src="/example/disabled_2.png" alt=""></div>
          <div class="disabled" :class="selectedImgIndex === 6 ? 'active': ''"><img id="6" src="/example/disabled_3.png" alt=""></div>
        </li>
        <li>
          <div class="disabled" :class="selectedImgIndex === 7 ? 'active': ''"><img id="7" src="/example/disabled_4.png" alt=""></div>
          <div class="disabled" :class="selectedImgIndex === 8 ? 'active': ''"><img id="8" src="/example/disabled_5.png" alt=""></div>
          <div class="disabled" :class="selectedImgIndex === 9 ? 'active': ''"><img id="9" src="/example/disabled_6.png" alt=""></div>
          <div class="disabled" :class="selectedImgIndex === 10 ? 'active': ''"><img id="10" src="/example/disabled_7.png" alt=""></div>
          <!-- <div :class="selectedImgIndex === 11 ? 'active': ''"><img id="11" src="/example/disabled_1.png" alt=""></div> -->
        </li>
      </ul>

      <div class="m-footer">
        <div class="m-line">
          <div class="m-left">
          </div>
          <div class="m-right" @click="pageNum = 0"><span>选择图案</span></div>
        </div>
        <div style="text-algin: right; margin-top: 48px">
          <img src="./assets/bg3.png" alt="">
        </div>
      </div>
    </div>

    <div v-show="!loading && pageNum === 2" class="m-example m-page-2">
      <img class="m-bg" src="./assets/bg.png" alt="">
      <div class="m-content">
        <div class="m-box-1">
          <div class="m-img">
            <img :src="white ? 'example/white_l.png' : 'example/black_l.png'" alt="">
            <h3><em>DIAMOND LOVE</em> #{{ white ? 'WHITE' : 'BLACK' }}</h3>
            <p><span>尺码-{{ selectedSize }}</span></p>
          </div>
        </div>
        <div class="m-box-2">
          <div class="m-form">
            <label for="name">姓名</label>
            <input id="name" v-model="username" type="text" @focus="canScan=false" @blur="canScan=true" spellcheck="false" />
            <label for="phone" >手机号</label>
            <input class="m-phone" type="text" v-model="phoneNum" @focus="canScan=false" @blur="checkPhone" spellcheck="false" />
            <span class="m-error-msg" :class="errorMsgVisible ? 'm-show': ''">号码不合法，请重新输入</span>
          </div>
        </div>
      </div>

      <div class="m-footer">
        <div class="m-line">
          <div class="m-left">
          </div>
          <div class="m-right" @click="submitTrade"><span>提交订单</span></div>
        </div>
        <div style="text-algin: right; margin-top: 48px">
          <img src="./assets/bg3.png" alt="">
        </div>
      </div>

      <div class="m-to-last" @click="pageNum = 0"><span>返回上页</span></div>
    </div>

    <div v-show="!loading && pageNum === 3" class="m-example m-page-3">
      <img class="m-bg" src="./assets/bg.png" alt="">
      <div class="m-content">
        <img src="./assets/ok.png" alt="">
        <p>订单提交成功</p>
      </div>

      <div class="m-footer">
        <div class="m-line">
          <div class="m-left">
          </div>
          <div class="m-right" @click="pageNum = 0"><span>返回首页</span></div>
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
div,
ul,
li {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

li {
  list-style: none;
}

::-webkit-scrollbar {
  display: none;
}

#app {
  width: 1920px;
  height: 100vh;
  overflow: hidden;
  background-color: #000;
}
#app video {
  width: 1920px;
  height: auto;
  vertical-align: middle;
}
em {
  font-family: AlibabaPuHuiTi-Light;
  font-style: normal;
  font-weight: 200;
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

.m-example .m-brand {
  display: flex;
  position: absolute;
  top: 433px;
  left: 861px;
  height: 50px;
  width: 962px;
  align-items: center;
}

.m-example .m-brand img {
  width: 68px;
  height: 30px;
  object-fit: cover;
  margin-right: 30px;
}

.m-example .m-brand p {
  width: 861px;
  height: 50px;
  font-size: 15px;
  font-family: AlibabaPuHuiTi-Regular, AlibabaPuHuiTi;
  font-weight: 400;
  color: rgba(255,255,255,0.6);
  line-height: 25px;
  -webkit-background-clip: text;
}

.m-example .m-info {
  position: absolute;
  padding: 69px 20px 20px 20px;
  left: 861px;
  top: 509px;
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
  z-index: 1000;
}
.m-footer .m-line {
  position: relative;
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
  margin-top: 50px;
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

.m-footer .m-price{
  display: flex;
  position: absolute;
  top: 0;
  left: 536px;
  height: 60px;
  align-items: center;
}

.m-footer .m-price span {
  margin-right: 20px;
  height: 30px;
  font-size: 22px;
  font-family: AlibabaPuHuiTi-Regular, AlibabaPuHuiTi;
  font-weight: 400;
  color: #FFFFFF;
  line-height: 30px;
  -webkit-background-clip: text;
}

.m-footer .m-price::before {
  display: block;
  content: "";
  width: 16px;
  height: 16px;
  margin-right: 15px;
  background-color: #fff;
}

.m-footer .m-price strong {
  height: 30px;
  font-size: 30px;
  font-family: AlibabaPuHuiTi-Bold, AlibabaPuHuiTi;
  font-weight: bold;
  color: #FFFFFF;
  line-height: 30px;
  -webkit-background-clip: text;
}

.m-page-1 .m-content {
  position: absolute;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  padding: 167px 92px;
  z-index: 100;
}

.m-page-1 .m-content li {
  display: flex;
  width: 100%;
  height: 300px;
  margin-bottom: 50px;
  align-items: center;
}
.m-page-1 .m-content li > div {   
  width: 300px;
  height: 300px;
  border-radius: 10px;
  overflow: hidden;
  background: linear-gradient(to bottom, #474747, #181818);
}
.m-page-1 .m-content li > div + div {
  margin-left: 65px;
}
.m-page-1 .m-content li > div img {
  width: 100%;
  user-select: none;
}
.m-page-1 .m-content li .active {
  border: 1px solid #fff;
}
.m-page-1 .m-content li .disabled {
  pointer-events: none;
}

.m-page-2 .m-content{
  position: absolute;
  top: 0;
  left: 0;
  padding: 215px 416px;
  width: 100vw;
  z-index: 100;
  display: flex;
  justify-content: space-between;
}
.m-page-2 .m-content .m-box-1 {
  width: 406px;
  height: 552px;
  background: url("./assets/border3.png");
}

.m-page-2 .m-content .m-box-2 {
  position: relative;
  padding: 130px;
  width: 588px;
  height: 552px;
  background: url("./assets/border4.png");
}
.m-page-2 .m-content .m-box-2::before {
  display: block;
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  width: 119px;
  height: 46px;
  background: url('./assets/deng_ji.png');
}
.m-page-2 .m-content .m-box-2 .m-form {
  text-align: center;

}
.m-page-2 .m-content .m-box-2 .m-form label {
  display: block;
  margin-bottom: 14px;
  height: 33px;
  font-size: 24px;
  font-family: AlibabaPuHuiTi-Medium, AlibabaPuHuiTi;
  font-weight: 500;
  color: #FFFFFF;
  line-height: 33px;
  letter-spacing: 2px;
  -webkit-background-clip: text;
}
.m-page-2 .m-content .m-box-2 .m-form input {
  display: block;
  margin-bottom: 95px;
  width: 100%;
  height: 50px;
  line-height: 50px;
  text-align: center;
  border: 1px solid #FFFFFF;
  background: rgba(255,255,255,0.1);
  outline: none;
  color: #FFFFFF;
  letter-spacing: 2px;
  font-size: 20px;
  font-weight: 400;
  font-family: AlibabaPuHuiTi-Regular, AlibabaPuHuiTi;
}
.m-page-2 .m-content .m-box-2 .m-form .m-phone {
  margin-bottom: 0;
}
.m-page-2 .m-content .m-box-2 .m-form .m-error-msg {
  width: 328px;
  height: 14px;
  font-size: 14px;
  font-family: AlibabaPuHuiTi-Light, HarmonyOS Sans SC;
  font-weight: 300;
  color: #FA5151;
  line-height: 16px;
  opacity: 0;
  -webkit-background-clip: text;
  transition: all 0.2s;
}

.m-page-2 .m-content .m-box-2 .m-form .m-show {
  opacity: 1;
}

.m-page-2 .m-content .m-box-1 .m-img {
  margin: 90px auto 27px;
  width: 300px;
  height: 300px;
}
.m-page-2 .m-content .m-box-1 .m-img h3 {
  margin-bottom: 15px;
  height: 25px;
  font-size: 18px;
  font-family: AlibabaPuHuiTi-Light, AlibabaPuHuiTi;
  font-weight: 300;
  color: #FFFFFF;
  line-height: 25px;
  text-align: center;
  -webkit-background-clip: text;
}
.m-page-2 .m-content .m-box-1 .m-img p {
  margin: 0 auto;
  width: 217px;
  height: 38px;
  line-height: 38px;
  background-color: #fff;
  text-align: center;
}
.m-page-2 .m-content .m-box-1 .m-img p span {
  font-size: 18px;
  font-family: Source Han Sans CN-Bold, Source Han Sans CN;
  font-weight: bold;
  color: #000000;
  letter-spacing: 3px;
  -webkit-background-clip: text;
}
.m-page-3 .m-content {
  position: absolute;
  top: 0;
  left: 0;
  padding: 215px;
  width: 100vw;
  text-align: center;
}
.m-page-3 .m-content p {
  padding-left: 68px;
  margin-top: 20px;
  height: 33px;
  font-size: 24px;
  font-family: AlibabaPuHuiTi-Medium, AlibabaPuHuiTi;
  font-weight: 500;
  color: #FFFFFF;
  line-height: 33px;
  letter-spacing: 2px;
  text-align: center;
  -webkit-background-clip: text;
}
.m-to-last {
  position: absolute;
  left: 110px;
  top: 902px;
  width: 160px;
  height: 60px;
  line-height: 60px;
  text-align: center;
  background-color: #fff;
  border-radius: 8px;
  cursor: pointer;
  z-index: 1000;
}
.m-to-last span {
  font-size: 30px;
  font-family: AlibabaPuHuiTi-Bold, AlibabaPuHuiTi;
  font-weight: bold;
  color: #000000;
  user-select: none;
  -webkit-background-clip: text;
}
.m-video-btn {
  position: absolute;
  left: 50px;
  top: 40px;
  width: 360px;
  height: 70px;
  background-color: #fff;
  z-index: 999999;
  opacity: 0;
}
</style>
