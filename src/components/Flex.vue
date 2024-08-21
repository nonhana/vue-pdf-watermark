<script setup lang="ts">
const numArr = new Array(16).fill(0).map((_, i) => i + 1);
</script>

<template>
  <div>
    <span>这是一段文字</span>
    <div class="container">
      <div class="container-item" v-for="num in numArr">
        <span>{{ num }}</span>
      </div>
    </div>
    <span>这是一段文字</span>
  </div>
</template>

<style scoped lang="scss">
.container {
  display: inline-flex;
  justify-content: center;
  align-content: stretch;
  width: 450px;
  height: 600px;
  flex-wrap: wrap;
  border: solid;
  &-item {
    width: 100px;
    height: 100px;
    background-color: #d3d311;
    display: flex;
    justify-content: center;
    align-items: center;
    span {
      font-size: 20px;
    }
  }
}
</style>
