<script setup lang="ts">
import * as PIXI from 'pixi.js'
// 创建应用
const app = new PIXI.Application({
  width: window.innerWidth, // 画布宽
  height: window.innerHeight, // 画布高
  background: 'black', // 画布背景颜色
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
// 将组件添加到pixi画布上
app.stage.addChild(sprite);
// 将精灵 进行360°旋转
// sprite.rotation = -Math.PI /4
// ticker添加动画，每一帧每一帧的执行回调函数
// app.ticker.add((delta) => {
//   console.log(delta); // delta每一帧的变化
//   sprite.rotation += 0.01 * delta;
// });

// 加载资源
// PIXI.Assets.add("Jian", './texture/jian.png')
// PIXI.Assets.add("Miss", './texture/miss1.png')

// 添加场景一的资源
PIXI.Assets.addBundle('scene1', {
  Jian: './texture/jian.png',
  Miss: './texture/miss1.png',
  Ma: './texture/ma.png',
})

// 异步加载资源
// const texturesPromise = PIXI.Assets.load(["Jian", "Miss"])
const texturesPromise = PIXI.Assets.loadBundle('scene1', (progress) => {
  console.log("加载完成进度", progress);
})
texturesPromise.then(texture => {
  // 创建一个容器
  const container = new PIXI.Container()
  // 创建精灵
  const Jian = new PIXI.Sprite(texture.Jian)
  // 设置精灵位置
  Jian.x = 200
  Jian.y = 200
  Jian.anchor.set(0.5, 0.5) // 设置精灵锚点
  Jian.scale.set(0.5) // 设置精灵缩放
  Jian.alpha = 1 // 设置精灵透明度
  Jian.rotation = 0.5 // 设置精灵旋转
  Jian.blendMode = PIXI.BLEND_MODES.ADD // 设置精灵混合模式
  Jian.eventMode = 'static'; // 设置精灵交互
  Jian.cursor = 'pointer'; // 设置精灵鼠标样式
  Jian.on("pointerdown", () =>{ // 设置精灵鼠标事件
    console.log("pointerdown");
  })
  // 创建精灵
  const Miss = new PIXI.Sprite(texture.Miss)
  // 设置精灵位置
  Miss.x = 350
  Miss.y = 200
  Miss.anchor.set(0.5, 0.5) // 设置精灵锚点
  Miss.scale.set(0.5) // 设置精灵缩放
  // app.stage.addChild(Jian, Miss);
  const Ma = new PIXI.Sprite(texture.Ma)
  // 设置精灵位置
  Ma.x = 500
  Ma.y = 200
  Ma.anchor.set(0.5, 0.5) // 设置精灵锚点
  Ma.scale.set(0.5) // 设置精灵缩放
  container.addChild(Jian, Miss, Ma);
  app.stage.addChild(container)
})

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
