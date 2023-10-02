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

// 绘制圆角矩形
const roundRectangle = new PIXI.Graphics();
roundRectangle.beginFill(0xcf5afc, 0.9);
// drawRoundedRect(x, y, width, height, radius)
roundRectangle.drawRoundedRect(200, 200, 50, 50, 5)
roundRectangle.endFill() 
app.stage.addChild(roundRectangle)

// 绘制椭圆
const ellipse = new PIXI.Graphics();
ellipse.beginFill(0xff0044);
ellipse.lineStyle(1, 0xffffff);
// 参数：cx, cy, width, height
ellipse.drawEllipse(300, 100, 80, 40);
ellipse.endFill() 
app.stage.addChild(ellipse);

// 绘制线段
const line = new PIXI.Graphics();
line.lineStyle(2, 'red');
line.moveTo(20, 50);
line.lineTo(100, 80);
line.lineTo(100, 50);

line.endFill() 
// 添加到 stage 下
app.stage.addChild(line);

// 绘制多边形
const polygon = new PIXI.Graphics();
polygon.beginFill(0x66ccff, 0.9)
// 绘制多边形，每两个点位一组，形成多连线的多边形图形
polygon.drawPolygon([0, 150, 100, 150, 40, 223, 20, 223])
polygon.endFill() 
app.stage.addChild(polygon);

// 绘制圆弧
const arc = new PIXI.Graphics();
arc.beginFill(0x000000, 0.9)
// arc(x,y, 半径, 起始角度, 结束角度, 是否顺时针)
arc.arc(400, 200, 50, 0, Math.PI, false)
arc.endFill()
app.stage.addChild(arc);
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
