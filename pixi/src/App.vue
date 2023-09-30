<script setup lang="ts">
import * as PIXI from 'pixi.js'
// 创建应用
const app = new PIXI.Application({
  width: window.innerWidth, // 画布宽
  height: window.innerHeight, // 画布高
  background: '#1099bb', // 画布背景颜色
  resolution: window.devicePixelRatio || 1
  // resizeTo: window
});

document.body.appendChild(app.view)

// 创建一个矩形
const rectangle = new PIXI.Graphics()
rectangle.beginFill('pink') // 填充矩形
rectangle.drawRect(100, 80, 80, 80) // 绘制矩形
rectangle.endFill() // 结束填充

// 将组件添加到pixi画布上
app.stage.addChild(rectangle);
</script>

<template>
  <canvas></canvas>
</template>

<style scoped>
* {
  margin: 0;
  padding: 0;
}

canvas {
  width: 100vw;
  height: 100vw;
  position: fixed;
  top: 0;
  left: 0;
}
</style>
