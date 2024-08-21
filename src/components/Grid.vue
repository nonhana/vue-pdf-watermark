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
  width: 1000px;
  height: 600px;
  border: solid;
  display: grid;
  grid-template-columns: repeat(4, 120px);
  grid-template-rows: repeat(4, 120px);
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
