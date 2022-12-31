<template>
  <div class="category" v-on:scroll="props.onScroll">
    <ul>
      <li v-for="item in props.list">
        <video :data-video-id="item.id" loop muted :src="item.src"></video>
      </li>
    </ul>
  </div>
</template>

<script setup lang="ts">
import { VideoData } from '../constants/data'
const props = defineProps<{
  list: VideoData[]
  onScroll: () => void
}>()
</script>
<style scoped lang="less">
.category {
  width: 100%;
  overflow: auto;
  ul {
    display: flex;
    padding: 16px;

    li {
      margin-right: 16px;

      &:last-child {
        margin-right: 0;
      }

      video {
        background-color: black;
        height: 240px;
        width: 136px;
        border-radius: 6px;
      }
    }
  }
}
</style>
