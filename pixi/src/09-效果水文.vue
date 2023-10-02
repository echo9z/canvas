<script setup lang="ts">
import * as PIXI from 'pixi.js'
import { ShockwaveFilter } from 'pixi-filters'
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
// 1.背景
// 创建一个纹理
const texture = PIXI.Texture.from("./texture/bg.jpg")
// 创建一个精灵
const bgc = new PIXI.Sprite(texture)
// 设置精灵图标的位置
bgc.width = app.screen.width// 画布宽度一半
bgc.height = app.screen.height
// sprite.anchor.set(0.5, 0.5)  // 设置精灵锚点

// app.stage.addChild(bgc)

// 2.文字
const text = new PIXI.Text('锦鲤', {
  fontFamily: 'Times New Roman',
  fontSize: 30 + Math.floor(app.screen.width / 10),
  fill: 0x000000,
  align: 'center',
  dropShadow: true, // 设置🎵
  dropShadowColor: '#c6d9f1', // 阴影颜色
  dropShadowBlur: 5, // 阴影模糊度
  dropShadowAngle: Math.PI / 2, // 阴影角度
  dropShadowDistance: 10, // 阴影厚度
})
// 设置文字在画布位置
text.x = app.screen.width / 2 // 画布宽度一半
text.y = app.screen.height / 2
text.anchor.set(0.5)

// 3.创建一个容器
const container = new PIXI.Container()
container.addChild(bgc)

// 4.加载资源
PIXI.Assets.addBundle('scene1', {
  White: './texture/white.png',
  Red: './texture/red.png',
})
const texturesPromise = PIXI.Assets.loadBundle('scene1', (progress) => {
  console.log("加载完成进度", progress);
})
texturesPromise.then(texture => {
  // 创建精灵
  const White = new PIXI.Sprite(texture.White)
  // 设置精灵位置
  White.x = 500
  White.y = 400
  White.anchor.set(0.5, 0.5) // 设置精灵锚点
  // White.scale.set(0.5) // 设置精灵缩放
  White.alpha = 1 // 设置精灵透明度
  White.rotation = 0.5 // 设置精灵旋转
  // White.blendMode = PIXI.BLEND_MODES.ADD // 设置精灵混合模式
  White.eventMode = 'static'; // 设置精灵交互
  White.cursor = 'pointer'; // 设置精灵鼠标样式
  White.on("click", () => { // 设置精灵鼠标事件
    console.log("pointerdown");
  })

  // 创建精灵
  const Red = new PIXI.Sprite(texture.Red)
  // 设置精灵位置
  Red.x = 1000
  Red.y = 550
  Red.anchor.set(0.5, 0.5) // 设置精灵锚点
  // Red.scale.set(0.5) // 设置精灵缩放

  // 将white和red精灵加入到容器中
  container.addChild(White, Red);
})

// 5.添加置换滤镜
const displacementSprite = PIXI.Sprite.from('./texture/displacement.png')
displacementSprite.scale.set(0.5)
// 设置纹理的重复模式 运行重复repeat
displacementSprite.texture.baseTexture.wrapMode = PIXI.WRAP_MODES.REPEAT
// 设置并创建滤镜
const displacementFilter = new PIXI.DisplacementFilter(displacementSprite)
container.addChild(displacementSprite)
// 6.设置文字滤镜
// container.filters = [displacementFilter]

// 7.将容器添加到画布上，这里添加元素是有先后顺序
app.stage.addChild(container, text)

// 添加水震波纹滤镜
// https://filters.pixijs.download/main/docs/ShockwaveFilter.html 具体参数
const shockwaveFilter1 = new ShockwaveFilter([
  Math.random() * app.screen.width,
  Math.random() * app.screen.height,
], {
  radius: 120,
  // speed: 3000,
  wavelength: 100, // 波长
  amplitude: 40,// 振幅
  brightness: 1
}, 0)
const shockwaveFilter2 = new ShockwaveFilter([
  Math.random() * app.screen.width,
  Math.random() * app.screen.height,
], {
  radius: 65,
  // speed: 3000,
  wavelength: 80, // 波长
  amplitude: 30,// 振幅
  brightness: 1
}, 0)
const shockwaveFilter3 = new ShockwaveFilter([
  Math.random() * app.screen.width,
  Math.random() * app.screen.height,
], {
  radius: 160,
  speed: 500,
  wavelength: 160, // 波长
  amplitude: 30,// 振幅
  brightness: 1 // 波出现的亮度
}, 0)
container.filters = [
  displacementFilter, 
  shockwaveFilter1,
  shockwaveFilter2, 
  shockwaveFilter3
]

// 曲面水滴效果，通过动画帧不断的改变纹理
app.ticker.add((delta) => {
  // console.log(delta);
  displacementSprite.x += 1;
  displacementSprite.y += 1;
  createWave(shockwaveFilter1, 2)
  createWave(shockwaveFilter2, 1.5)
  createWave(shockwaveFilter3, 3)
})

function createWave(waveFilter: ShockwaveFilter, resetTime: number) {
  waveFilter.time += 0.01 // 每一次加起时间就会有波浪扩散起来
  if(waveFilter.time > resetTime) { // 如果波浪的时间超过设定时间
    waveFilter.time = 0
    waveFilter.center = [
      Math.random() * app.screen.width,
      Math.random() * app.screen.height,
    ]
  }
}
// 监听点击事件，根据位置创建波纹
app.view.addEventListener("click", (e) => {
  shockwaveFilter3.center = [e.clientX, e.clientY]
  shockwaveFilter3.time = 0
})
// app.view.addEventListener("click", (e) => {
//   console.log(e.clientX, e.clientY);
//   const shockwaveFilter = new ShockwaveFilter([
//     e.clientX,
//     e.clientY,
//   ], {
//     radius: 160,
//     speed: 500,
//     wavelength: 160, // 波长
//     amplitude: 30,// 振幅
//     brightness: 1 // 波出现的亮度
//   }, 0)
//   container.filters?.push(shockwaveFilter)

//   app.ticker.add(() => {
//     createWave(shockwaveFilter, 2)
//   })
// })

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
