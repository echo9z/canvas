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

// 显示文字
const text = new PIXI.Text('Hello world', {
  fontFamily: 'Arial',
  fontSize: 200,
  fill: 0xff0000,
  align: 'center',
})
// 设置文字在画布位置
text.x = app.screen.width / 2 // 画布宽度一半
text.y = app.screen.height / 2
text.anchor.set(0.5)
app.stage.addChild(text)

const circle = new PIXI.Graphics();
circle.beginFill(0x00FF44); // 填充颜色
// circle.lineStyle(1, 0xFFFFFF, 1); // 线宽 颜色 透明度
// 参数：cx（中点）, cy, radius
circle.drawCircle(100, 100, 50);
circle.endFill() 
app.stage.addChild(circle)

// 创建一个精灵
const bgc = PIXI.Sprite.from("./texture/bg.jpg")
// 用精灵铺满整个屏幕
bgc.width = app.screen.width
bgc.height = app.screen.height

const ba = PIXI.Sprite.from("./texture/bg.jpg")
// // 用精灵铺满整个屏幕
ba.width = 500
ba.height = 500

// 使用文件作为精灵的遮罩
bgc.mask = text
ba.mask = circle

// 使用文件作为精灵的遮罩
app.stage.addChild(bgc, ba)

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
