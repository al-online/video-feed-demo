<template>
  <nav class="navBar">
    <span class="icon"><left-outlined /></span>
    <span>{{ props.title }}</span>
  </nav>
</template>

<script setup lang="ts">
import { LeftOutlined } from '@ant-design/icons-vue'
const props = defineProps<{
  title: string
}>()
</script>
<style scoped lang="less">
.navBar {
  width: 100%;
  height: 56px;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #fb7299;
  .icon {
    position: absolute;
    left: 16px;
    top: 28%;
    transform: translateY(-50%);
  }
}
</style>
