<template>
  <div class="container">
    <el-button
      @click="
        showPdfWindow(
          'https://cdn.jsdelivr.net/gh/themusecatcher/resources@0.0.3/Markdown.pdf'
        )
      "
      >打开弹窗</el-button
    >

    <el-dialog width="80%" v-model="dialogVisible" title="pdf预览">
      <div class="pdf-container">
        <canvas id="pdf-canvas"></canvas>
      </div>
      <div class="pagination-controls">
        <el-button @click="prevPage" :disabled="currentPage === 1"
          >上一页</el-button
        >
        <span>{{ currentPage }} / {{ pdfPages }}</span>
        <el-button @click="nextPage" :disabled="currentPage >= pdfPages"
          >下一页</el-button
        >
      </div>
    </el-dialog>
  </div>
</template>

<script setup lang="ts">
import { ref, nextTick, watch } from "vue";
import * as PDFJS from "pdfjs-dist";

const dialogVisible = ref(false);

const pdfSrc = ref<string>("");
let pdfDoc: any = null;
const pdfPages = ref(0);
const pdfScale = ref(1.5);
const currentPage = ref(1);

/* ----------水印相关---------- */
const watermark = ref<string>("Sample Watermark");

const initWatermark = () => {
  let canvas = document.createElement("canvas");
  canvas.width = 200;
  canvas.height = 200;

  let ctx = canvas.getContext("2d");
  if (!ctx) return canvas;

  ctx.rotate((-18 * Math.PI) / 180);
  ctx.font = "14px Vedana";
  ctx.fillStyle = "rgba(200, 200, 200, 0.3)";
  ctx.textAlign = "left";
  ctx.textBaseline = "middle";
  ctx.fillText(watermark.value, 50, 50);

  return canvas;
};

const renderWatermark = (canvas: HTMLCanvasElement) => {
  const ctx = canvas.getContext("2d");
  if (!ctx) return;

  let watermarkCanvas = initWatermark();
  if (!watermarkCanvas) return;

  let pattern = ctx.createPattern(watermarkCanvas, "repeat");
  if (!pattern) return;

  ctx.rect(0, 0, canvas.width, canvas.height);
  ctx.fillStyle = pattern;
  ctx.fill();
};

/* ----------PDF 相关---------- */
// 加载pdf文件
const loadFile = async (url: string) => {
  PDFJS.GlobalWorkerOptions.workerSrc = `https://cdnjs.cloudflare.com/ajax/libs/pdf.js/${PDFJS.version}/pdf.worker.min.mjs`;
  const response = await fetch(url);
  const arrayBuffer = await response.arrayBuffer();

  const loadingTask = PDFJS.getDocument(arrayBuffer);
  loadingTask.promise
    .then(async (pdf: any) => {
      pdf.loadingParams.disableAutoFetch = true;
      pdf.loadingParams.disableStream = true;
      pdfDoc = pdf; // 保存加载的pdf文件流
      pdfPages.value = pdfDoc.numPages; // 获取pdf文件的总页数
      currentPage.value = 1; // 初始化为第一页
      await nextTick(() => {
        renderPage(currentPage.value); // 渲染第一页
      });
    })
    .catch((error: any) => {
      console.warn(`pdfReader loadFile error: ${error}`);
    });
};

// 渲染当前页面
const renderPage = (num: number): void => {
  pdfDoc.getPage(num).then((page: any) => {
    const canvas: any = document.getElementById("pdf-canvas"); // 获取页面中的canvas元素
    // 以下canvas的使用过程
    const ctx: any = canvas.getContext("2d");
    const dpr = window.devicePixelRatio || 1;
    const bsr =
      ctx.webkitBackingStorePixelRatio ||
      ctx.mozBackingStorePixelRatio ||
      ctx.msBackingStorePixelRatio ||
      ctx.oBackingStorePixelRatio ||
      ctx.backingStorePixelRatio ||
      1;
    const ratio = dpr / bsr;
    const viewport = page.getViewport({ scale: pdfScale.value }); // 设置pdf文件显示比例
    canvas.width = viewport.width * ratio;
    canvas.height = viewport.height * ratio;
    canvas.style.width = viewport.width + "px";
    canvas.style.height = viewport.height + "px";
    ctx.setTransform(ratio, 0, 0, ratio, 0, 0); // 设置当pdf文件处于缩小或放大状态时，可以拖动
    const renderContext = {
      canvasContext: ctx,
      viewport: viewport,
    };
    // 将pdf文件的内容渲染到canvas中
    page.render(renderContext).promise.then(() => {
      // PDF渲染完成后，添加水印
      renderWatermark(canvas);
    });
  });
};

// 打开弹窗
const showPdfWindow = (src: string) => {
  pdfSrc.value = src;
};

watch(pdfSrc, () => {
  if (pdfSrc.value) {
    loadFile(pdfSrc.value);
    dialogVisible.value = true;
  }
});

// 下一页
const nextPage = () => {
  if (currentPage.value < pdfPages.value) {
    currentPage.value++;
    renderPage(currentPage.value);
  }
};

// 上一页
const prevPage = () => {
  if (currentPage.value > 1) {
    currentPage.value--;
    renderPage(currentPage.value);
  }
};
</script>

<style scoped>
.pdf-container {
  position: relative;
  width: 100%;
  overflow: hidden;
  display: flex;
  justify-content: center;
  align-items: center;
}

.pagination-controls {
  display: flex;
  justify-content: center;
  margin-top: 10px;
}

canvas {
  border: 1px solid #ccc;
  margin: 0 auto;
}
</style>
