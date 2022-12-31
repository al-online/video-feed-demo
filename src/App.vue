<template>
  <div class="wrapper">
    <header class="header" :class="{ hidden: hidden }">
      <NavBar title="首页" />
      <Tabs />
    </header>

    <div class="line"></div>

    <div class="scrollView" v-on:scroll="OnScroll">
      <img class="banner" src="/banner.png" alt="" />
      <div class="content" ref="refContent">
        <h2>热门</h2>
        <Category :onScroll="OnScroll" :list="dataSource.hot.list" />
        <h2>直播</h2>
        <Category :onScroll="OnScroll" :list="dataSource.live.list" />
        <h2>推荐</h2>
        <Category :onScroll="OnScroll" :list="dataSource.recommend.list" />
      </div>

      <img class="banner" src="/footer.jpg" alt="" />
      <footer class="footer">
        <span>@Bilibili 2022</span>
      </footer>
    </div>
  </div>
</template>

<script setup lang="ts">
import Category from './components/Category.vue'
import NavBar from './components/NavBar.vue'
import Tabs from './components/Tabs.vue'
import { debounce } from 'lodash'
import { dataSource } from './constants/data'
import { onMounted, ref } from 'vue'
const refContent = ref<HTMLDivElement>()
const oldY = ref<number>(0)
const playingIds = ref<string[]>([])

const isScrolling = ref<boolean>(false)

//是否隐藏
const hidden = ref<boolean>(false)
const playAll = (ids: string[]) => {
  if (ids.length === 0) {
    return
  }

  const selector = ids.map((id) => `[data-video-id="${id}"]`).join(',')
  const videoEls: HTMLVideoElement[] = Array.from(document.querySelectorAll(selector))

  videoEls.forEach((el) => el.play())

  playingIds.value = ids
}

const stopAll = (ids: string[]) => {
  if (ids.length === 0) {
    return
  }

  const selector = ids.map((id) => `[data-video-id="${id}"]`).join(',')
  const videoEls: HTMLVideoElement[] = Array.from(document.querySelectorAll(selector))

  videoEls.forEach((el) => {
    el.pause()
    el.currentTime = 0
  })
}

const pauseAll = (ids: string[]) => {
  if (ids.length === 0) {
    return
  }

  const selector = ids.map((id) => `[data-video-id="${id}"]`).join(',')
  const videoEls: HTMLVideoElement[] = Array.from(document.querySelectorAll(selector))

  videoEls.forEach((el) => el.pause())
}

const scrollEnd = debounce(() => {
  const videoEls = Array.from(document.querySelectorAll('video'))

  // 找到命中规则的视频
  const inViewVideoEls = videoEls.filter((el) => isInView(el))

  if (inViewVideoEls.length > 0) {
    const ids: string[] = inViewVideoEls.map((el) => el.getAttribute('data-video-id') || '')

    // 旧视频（以前的 Id 不在这次播放列表中的）
    const stopIds = playingIds.value.filter((id) => !ids.includes(id))
    stopAll(stopIds)

    // 播放新视频
    playAll(ids)
  } else {
    playAll(playingIds.value)
  }
  isScrolling.value = false
}, 500)

const isInView = (el: HTMLVideoElement) => {
  const { top, bottom, left, right } = el.getBoundingClientRect()

  // 水平方向
  const isHorizontalInView = 0 < left && right < window.innerWidth
  // 垂直方向
  const isVerticalInView = top < window.innerHeight / 2 && window.innerHeight / 2 < bottom
  // 最终结果
  return isHorizontalInView && isVerticalInView
}

const OnScroll = () => {
  //一开始滚动就停止所有视频
  if (isScrolling.value!) {
    pauseAll(playingIds.value)
  }

  isScrolling.value = true

  if (refContent.value) {
    const { top: newY } = refContent.value.getBoundingClientRect()
    const delta = newY - oldY.value

    if (delta < 0) {
      hidden.value = true
    } else {
      hidden.value = false
    }
  }
  scrollEnd()
}

onMounted(() => {
  const initVideoIds = dataSource.hot.list.slice(0, 2).map((item) => item.id)
  playAll(initVideoIds)
})
</script>
<style scoped lang="less">
.wrapper {
  background-color: #fb7299;
  color: white;
  .banner {
    width: 100vw;
  }

  .banner:first-child {
    margin-top: 100px;
  }

  .header {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    transform: translateY(0);
    transition: transform 250ms;
    &.hidden {
      transform: translateY(-56px);
    }
  }

  .footer {
    width: 100%;
    height: 56px;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  h2 {
    margin-top: 120px;
  }

  .content {
    padding: 10px;
  }

  .line {
    position: fixed;
    top: 50%;
    left: 0;
    width: 100%;
    height: 4px;
    background-color: red;
  }

  .scrollView {
    height: 100vh;
    overflow: auto;
  }
}
</style>
