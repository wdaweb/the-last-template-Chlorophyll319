<template>
  <v-container>
    <v-row>
      <v-col cols="12">
        <hr>
        <h2 style="margin: 10px;text-align: center;">念念不住，心安即定。<br>
          一刻專注，如坐禪——時間靜止，心無旁騖。</h2>
        <hr>
        <!-- 動畫文字區塊 -->
        <div class="container" style="margin: 10px; display: inline-block;text-align: right;">
          <h1 class="split">
            “Be present. Or at least pretend to be for 25 minutes.”
            The Pomodoro Technique: Fake it until the timer dings.
          </h1>
          <v-btn
            style="margin: 10px;"
            @click="replayAnimation"
          >Replay Slowly</v-btn>
        </div>

        <h1>目前事項</h1>

        <div style="margin: 10px;">
          <h2>{{ list.currentItem }}</h2>
          <h2>{{ list.timeleft }}</h2>
          <h2>{{ timeLeftText }}</h2>
          <DigitNumber
            v-for="(data, i) in timeLeftText"
            :key="i"
            color="#fff"
            :data="data"
          />
        </div>

      </v-col>

      <v-col cols="12">
        <v-btn
          :disabled="status === STATUS.COUNTING || (list.currentItem.length === 0 && list.items.length === 0)"
          icon="mdi-play"
          @click="startTimer"
        />
        <v-btn
          :disabled="status !== STATUS.COUNTING"
          icon="mdi-pause"
          @click="pause"
        />
        <v-btn
          :disabled="list.currentItem.length === 0"
          icon="mdi-skip-next"
          @click="finish"
        />

      </v-col>
    </v-row>

    <v-row>
      <v-col cols="12">
        <word />
        <theSwiper />
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup>
import { useWebNotification } from '@vueuse/core'
import { computed, onMounted, ref } from 'vue'

import DigitNumber from '@/components/DigitNumber.vue'
import theSwiper from '@/components/theSwiper.vue'
import word from '@/components/word.vue'
import { useListStore } from '@/stores/list'
import { useSettingsStore } from '@/stores/settings'

const list = useListStore()
const settings = useSettingsStore()

const STATUS = {
  STOP: 0,
  COUNTING: 1,
  PAUSE: 2,
}
const status = ref(STATUS.STOP)
let timer = 0

const startTimer = () => {
  if (status.value === STATUS.STOP && list.items.length > 0) {
    list.setCurrentItem()
  }
  status.value = STATUS.COUNTING
  timer = setInterval(() => {
    list.countdown()
    if (list.timeleft < 0) finish()
  }, 1000)
}

const pause = () => {
  clearInterval(timer)
  status.value = STATUS.PAUSE
}

const finish = () => {
  clearInterval(timer)
  status.value = STATUS.STOP
  const audio = new Audio()
  audio.src = settings.selectedAlarm.file
  audio.play()
  const { show, isSupported } = useWebNotification({
    title: '事項完成',
    body: list.currentItem,
    icon: new URL('@/assets/tomato.png', import.meta.url).href,
  })
  if (isSupported.value) show()
  list.setFinishItem()
  if (list.items.length > 0) startTimer()
}

const timeLeftText = computed(() => {
  const m = Math.floor(list.timeleft / 60).toString().padStart(2, '0')
  const s = (list.timeleft % 60).toString().padStart(2, '0')
  return m + ':' + s
})

// ✅ SplitText CDN 使用
let split
const replayAnimation = () => {
  if (split) {
    split.restart()
    split.timeScale(0.2)
  }
}

onMounted(async () => {
  await document.fonts.ready

  const gsap = window.gsap
  const SplitText = window.SplitText

  if (!gsap || !SplitText) {
    console.error('GSAP or SplitText not loaded')
    return
  }

  gsap.set('.split', { opacity: 1 })

  // 這裡改用 new SplitText
  const splitInstance = new SplitText('.split', {
    type: 'lines',
    linesClass: 'line',
  })
  split = gsap.from(splitInstance.lines, {
    duration: 0.6,
    yPercent: 100,
    opacity: 0,
    stagger: 0.1,
    ease: 'expo.out',
    paused: true, // 先暫停，等 replayAnimation 時才播放
  })
  split.play(0)
})
</script>

<style>
.container {
  max-width: 80vw;
}

.split {
  opacity: 0;
  text-align: center;
  color: #fff9e6;
  font-size: clamp(2rem, 5rem, 3vw);
  letter-spacing: 0.05rem;
  will-change: transform;
}

.split * {
  will-change: transform;
}

.swiper {
  width: 100%;
  height: 100%;
}

.swiper-slide {
  font-size: 18px;
  height: auto;
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
  padding: 30px;
}

</style>
