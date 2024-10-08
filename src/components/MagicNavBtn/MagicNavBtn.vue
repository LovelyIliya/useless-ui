<template>
  <button
    class="magic-nav-btn"
    ref="container"
    @click="handleClick"
    :class="{
      playing: playing && !showed,
      normalcy: showed && !playing,
      hideing: playing && showed
    }"
    :style="{ width: boxWidth }"
  >
    <span class="text">{{ props.text }}</span>
    <div class="icon-btn">
      <span class="iconfont icon-jia"></span>
    </div>
  </button>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const props = defineProps({
  text: {
    type: String,
    default: ''
  }
})

const container = ref()
const playing = ref(false)
const showed = ref(false)
const boxWidth = ref('55px')
function play() {
  if (playing.value || showed.value) return
  playing.value = true
  setTimeout(() => {
    boxWidth.value = 100 + props.text?.length * 15 + 'px'
  }, 700)
  setTimeout(() => {
    playing.value = false
    showed.value = true
  }, 1000)
}

function hide() {
  if (playing.value || !showed.value) return
  playing.value = true
  setTimeout(() => {
    boxWidth.value = '55px'
  }, 300)
  setTimeout(() => {
    playing.value = false
    showed.value = false
  }, 1000)
}

const emit = defineEmits(['click'])
function handleClick(e: Event) {
  emit('click', e)
}

defineExpose({ play, hide })
</script>

<style lang="scss" scoped>
.magic-nav-btn {
  border: 0 solid #0c77e4;
  backdrop-filter: blur(4px);
  background-color: rgba($color: #fff, $alpha: 0.7);
  scale: 0;
  border-radius: 30px;
  box-sizing: content-box;
  display: flex;
  align-items: center;
  transition: width 0.3s ease-in-out;
  // 常态样式
  &.normalcy {
    height: 55px;
    justify-content: space-between;
    scale: 1;
    .text {
      width: calc(100% - 50px);
      opacity: 1;
    }
    .icon-btn {
      margin-right: 10px;
      scale: 1;
    }
  }
  &.playing {
    height: 55px;
    width: 55px;
    border-width: 15px;
    border-radius: 50%;
    justify-content: center;
    animation:
      anim-show 0.5s ease-in-out forwards,
      anim-border 0.15s ease-in-out forwards 0.55s,
      anim-adjust 0.3s ease-in-out forwards 0.7s;
    .icon-btn {
      animation: anim-show 0.5s ease-in-out forwards 0.45s;
    }
  }
  &.hideing {
    height: 55px;
    scale: 1;
    padding-right: 0;
    animation:
      anim-adjust 0.2s ease-in-out forwards reverse 0.3s,
      anim-show 0.2s ease-in-out forwards 0.65s reverse;
    .text {
      width: calc(100% - 50px);
      opacity: 0;
    }
    .icon-btn {
      animation: anim-show 0.2s ease-in-out forwards reverse;
    }
  }
  .text {
    color: #1d1d1f;
    letter-spacing: 1px;
    opacity: 0;
    width: 0;
    text-align: center;
    transition: opacity 0.2s ease-in;
  }
  .icon-btn {
    width: 40px;
    height: 40px;
    line-height: 40px;
    scale: 0;
    border-radius: 50%;
    background-color: #0c77e4;
    color: #fff;
    text-align: center;
    transform-origin: center;
    .iconfont {
      font-size: 32px;
    }
  }
}

@keyframes anim-show {
  from {
    scale: 0;
  }
  to {
    scale: 1;
  }
}

@keyframes anim-border {
  from {
    border-radius: 50%;
    border-width: 15px;
  }
  to {
    border-radius: 30px;
    border-width: 0;
  }
}

@keyframes anim-adjust {
  0% {
    border-radius: 30px;
    justify-content: center;
    padding-right: 0;
  }
  10% {
    justify-content: space-between;
  }
  100% {
    padding-right: 10px;
    justify-content: space-between;
  }
}
</style>
