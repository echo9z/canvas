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

// 创建一个纹理
const texture = PIXI.Texture.from("./texture/bunny.png")
// 创建一个精灵
const sprite = new PIXI.Sprite(texture)
sprite.anchor.set(0.5, 0.5)
sprite.scale.set(2, 2)
sprite.alpha = 0.9
// 设置精灵图标的位置
sprite.x = app.screen.width / 2 // 画布宽度一半
sprite.y = app.screen.height / 2

// 将精灵 进行360°旋转
sprite.rotation = Math.PI /4
// ticker实现动画，每一帧每一帧的执行回调函数
app.ticker.add((delta) => {
  console.log(delta); // delta每一帧的变化
  sprite.rotation += 0.1 * delta;
});

app.stage.addChild(sprite)

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
