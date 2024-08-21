<template>
  <div ref="scrollContainer" class="virtual-list" @scroll="handleScroll">
    <div :style="{ height: `${totalHeight}px`, position: 'relative' }">
      <div
        v-for="item in visibleItems"
        :key="item.id"
        class="list-item"
        :style="{ top: `${item.top}px`, position: 'absolute', width: '100%' }"
      >
        {{ item.text }}
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref, onMounted, computed } from "vue";

const items = ref<Array<{ id: number; text: string; top: number }>>([]);
const scrollContainer = ref<HTMLElement | null>(null);
const itemHeight = 30; // 假设每个项的高度是30px
const visibleCount = ref(10); // 可视区域可以显示的项数

const totalHeight = computed(() => items.value.length * itemHeight);
const visibleItems = ref(items.value.slice(0, visibleCount.value));

onMounted(() => {
  for (let i = 0; i < 1000; i++) {
    items.value.push({ id: i, text: `Item ${i}`, top: i * itemHeight });
  }
  updateVisibleItems(0);
});

const handleScroll = () => {
  const scrollTop = scrollContainer.value?.scrollTop || 0;
  updateVisibleItems(scrollTop);
};

const updateVisibleItems = (scrollTop: number) => {
  const startIdx = Math.floor(scrollTop / itemHeight);
  const endIdx = startIdx + visibleCount.value;
  visibleItems.value = items.value
    .slice(startIdx, endIdx)
    .map((item, index) => ({
      ...item,
      top: (startIdx + index) * itemHeight,
    }));
};
</script>

<style>
.virtual-list {
  overflow-y: auto;
  height: 300px; /* 设置虚拟列表的高度 */
  width: 100%; /* 宽度根据需要设置 */
  position: relative;
}

.list-item {
  height: 30px; /* 同 itemHeight */
  line-height: 30px;
  text-align: center;
  border-bottom: 1px solid #ccc;
}
</style>
