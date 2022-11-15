<script lang="ts" setup>
  import { watch, ref } from 'vue'
  const props = defineProps<{ loading: boolean, wait: number }>()
  const emits =  defineEmits([ 'change' ])

  watch(
    () => props.loading,
    (newVal) => {
      if (newVal) {
        setTimeout(() => emits('change'), props.wait)
      }
    },
    {
      immediate: true
    }
  )
</script>

<template>
  <div class="m-box">
    <img class="m-bg" src="../assets/loading_bg.png" alt="">
    <div class="m-loading-box">
      <div class="m-ring"></div>
    </div> 
  </div>
</template>

<style>
.m-box {
  padding: relative;
  height: 100%;
  width: 1920px;
  background-color: #000;
}
.m-box .m-bg {
  position: absolute;
  top: 0;
  left: 0;
  width: 1920px;
  -webkit-user-drag: none;
  vertical-align: middle;
}
.m-loading-box {
  position: absolute;
  left: 960px;
  bottom: 173px;
  transform: translateX(-50%);
  background: url("../assets/loading.png");
  width: 108px;
  height: 108px;
  z-index: 100;
}
.m-ring {
  width: 108px;
  height: 108px;
  border: 16px solid #fff;
  border-radius: 50%;
  clip-path: polygon(0 0, 50% 0, 50% 100%, 0 100%);
  box-sizing: border-box;
  transform: rotate(0deg);
}
</style>