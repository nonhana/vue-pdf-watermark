<template>
  <div class="bfc-container">
    <div class="normal-box">
      <p>非 BFC 区域内的两个块元素</p>
      <div class="box">Box 1</div>
      <div class="box">Box 2</div>
    </div>
    <div class="bfc-box">
      <p>BFC 区域内的两个块元素</p>
      <div class="box">Box 1</div>
      <div class="box">Box 2</div>
    </div>
  </div>
</template>

<script setup lang="ts"></script>

<style>
.bfc-container {
  display: flex;
  justify-content: space-around;
  padding: 20px;
}

.normal-box,
.bfc-box {
  width: 200px;
}

.box {
  width: 100%;
  height: 100px;
  background-color: lightblue;
  border: 1px solid blue;
  margin: 20px;
}

.bfc-box {
  overflow: hidden; /* Creating a new BFC */
}
</style>
