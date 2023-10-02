<script setup lang="ts">
import * as PIXI from 'pixi.js'
// 创建应用
const app = new PIXI.Application({
  width: window.innerWidth, // 画布宽
  height: window.innerHeight, // 画布高
  background: '#1099bb', // 画布背景颜色
  resolution: window.devicePixelRatio || 1,
  antialias: true, // 设置抗锯齿
  // resizeTo: window
});

document.body.appendChild(app.view as any)

// 创建一个矩形
const rectangle = new PIXI.Graphics()
rectangle.beginFill('pink', 0.8) // 填充矩形，透明度
rectangle.drawRect(50, 50, 80, 80) // 绘制矩形
rectangle.endFill() // 结束填充

// 图像的缩放
// rectangle.scale.set(2, 2)
// 图像的位移 与canvas的translate基准点为100,100
rectangle.position.set(100, 100)
// 图像旋转
rectangle.rotation = 0.5 // 旋转45°，基准点以0,0旋转；
// 锚点，将矩形的旋转点以40,40做为中心点旋转
rectangle.pivot.set(40, 40)

// 绘制一个圆
const circle = new PIXI.Graphics();
circle.beginFill(0x00FF44); // 填充颜色
circle.lineStyle(1, 0xFFFFFF, 1); // 线宽 颜色 透明度
// 参数：cx（中点）, cy, radius
circle.drawCircle(100, 100, 5);
circle.endFill() 

// 将组件添加到pixi画布上
app.stage.addChild(rectangle, circle);
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
