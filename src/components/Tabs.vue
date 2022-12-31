<template>
  <div class="tabs">
    <li>视频</li>
    <li>动态</li>
    <li>娱乐</li>
  </div>
</template>

<script setup lang="ts"></script>
<style scoped lang="less">
.tabs {
  width: 100%;
  background-color: white;
  padding: 10px;
  display: flex;
  align-items: center;
  li {
    height: 30px;
    margin-right: 8px;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    padding: 0px 16px;
    color: gray;
    background-color: #ddd;
    border-radius: 50px;
  }
  li:first-child {
    background-color: #00aeec;
    color: white;
  }
}
</style>
