<script setup>
import FallingStarsBg from "./components/FallingStarsBg.vue"
import ShimmerButton from "./components/ShimmerButton.vue"
import TextHighlight from "./components/TextHighlight.vue"
import SmoothCursor from "./components/SmoothCursor.vue"
import PatternBackground from "./components/PatternBackground.vue"
import World from "./components/World.vue"
import Bottom from "./components/Bottom.vue"
import { MessagePlugin } from 'tdesign-vue-next'
import {
  PATTERN_BACKGROUND_DIRECTION,
  PATTERN_BACKGROUND_SPEED,
  PATTERN_BACKGROUND_VARIANT,
} from "./components";


const downloadFunc = (t) => {
  switch (t) {
    case "win":
      // MessagePlugin.success("马上下载")
      downloadFile("https://c.0a0a.cn/api/download", "穿云箭.exe")
      return
    case "mac":
      MessagePlugin.warning("开发中")
      return
  }
}

const downloadFile = (url, filename) => {
  const link = document.createElement('a')
  link.href = url
  link.download = filename
  document.body.appendChild(link)
  link.click()
  document.body.removeChild(link)
}


</script>

<template>
  <div style="scroll-behavior: smooth;">
    <SmoothCursor />
    <div class="relative flex flex-col items-center justify-center h-dvh">
      <FallingStarsBg class="bg-white" color="#555" />
      <div class="z-[1] flex items-center">
        <span class="text-6xl font-bold text-black dark:text-white">穿云箭</span>
      </div>
      <div class="z-[1] flex items-center">
        <div class="text-blance flex justify-center p-10">
          <h1 class="text-4xl font-bold">
            一款
            <TextHighlight class="rounded bg-gradient-to-r from-green-300 to-blue-300">
              免费、开源、免登录的内网穿透神器
            </TextHighlight>
          </h1>
        </div>
      </div>
      <div class="relative flex my-10">
        <ShimmerButton @click="downloadFunc('win')" background="#00A4EF" class="shadow-2xl mx-5" shimmer-size="2px">
          <span
            class="whitespace-pre-wrap text-center text-sm font-medium leading-none tracking-tight text-white lg:text-lg dark:from-white dark:to-slate-900/10">
            Win 下载
          </span>
        </ShimmerButton>
        <ShimmerButton @click="downloadFunc('mac')" background="#1D1D1F" class="shadow-2xl mx-5" shimmer-size="2px">
          <span
            class="whitespace-pre-wrap text-center text-sm font-medium leading-none tracking-tight text-white lg:text-lg dark:from-white dark:to-slate-900/10">
            Mac 下载
          </span>
        </ShimmerButton>
      </div>
    </div>
    <div class="relative flex flex-col items-center justify-center h-dvh">
      <PatternBackground :animate="true" :direction="PATTERN_BACKGROUND_DIRECTION.TopRight"
        :variant="PATTERN_BACKGROUND_VARIANT.Dot" class="flex h-[36rem] w-full items-center justify-center"
        :speed="PATTERN_BACKGROUND_SPEED.Slow">
        <p
          class="relative z-20 bg-gradient-to-b from-neutral-200 to-neutral-500 bg-clip-text py-8 text-4xl font-bold text-transparent sm:text-5xl">
          真实的页面
        </p>
      </PatternBackground>
      <World />
    </div>
    <Bottom />
  </div>
</template>
