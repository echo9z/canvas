<script setup lang="ts">
import * as PIXI from 'pixi.js'
import { PixelateFilter, GlowFilter } from 'pixi-filters'
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
const texture = PIXI.Texture.from("./texture/ba.png")
// 创建一个精灵
const sprite = new PIXI.Sprite(texture)
// 设置精灵图标的位置
sprite.x = app.screen.width / 2 // 画布宽度一半
sprite.y = app.screen.height / 2
sprite.anchor.set(0.5, 0.5)  // 设置精灵锚点

app.stage.addChild(sprite)

// 创建模糊滤镜
const blurFilter = new PIXI.BlurFilter()
// 设置模糊程度
blurFilter.blur = 10
// 将模糊滤镜添加到精灵上
sprite.filters = [blurFilter]

sprite.eventMode = 'static'
sprite.on('pointerenter', () => {
  blurFilter.blur = 0
})
sprite.on("pointerleave", () => {
  blurFilter.blur = 10
})

const Miss = PIXI.Sprite.from('./texture/miss1.png')
app.stage.addChild(Miss)
// 创建轮廓滤镜
// 需要安装 npm install pixi-filters
// 具体看 https://pixijs.io/filters/docs/
// 创建像素风格滤镜
const outlineFilter = new PixelateFilter(5)
const glowFilter = new GlowFilter({
  distance: 50,
  outerStrength: 6, // 缘向外发光的强度。
  color: 0x0000000
})
Miss.eventMode = 'static'
Miss.on('pointerenter', () => {
  outlineFilter.size = 1
})
Miss.on("pointerleave", () => {
  outlineFilter.size = 5
})
Miss.filters = [outlineFilter, glowFilter]
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
