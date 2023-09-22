# Canvas

## 认识canvas

画布canvas是H5中一个重要的概念，它面向开发人员提供了非常底层的绘图接口，使得绘制速度可以大幅提高，这对游戏等领域极为重要。

canvas看起来和 img 元素很相像，唯一的不同就是它并没有 src 和 alt 属性。实际上，canvas 标签只有两个属性---- width和height。

创建一个简单的画布

canvas初始化默认的width:300px;  height:150px

```js
  <!-- 
    id: 标识元素的唯一性
    width：画布宽度
    height：画布的高度
  -->
  <canvas id="emerge" width="600" height="400" ></canvas>
  <script>
    // 1.找到canvas对象
    var c1 = document.getElementById("emerge");
    // 2.获取画笔，上下文对象
    var ctx = c1.getContext("2d");
    // 3.绘制图形
    // 3.1 绘制矩形 fillRect(x, y, width, height)
    ctx.fillRect(100, 200, 300, 300)
  </script>
```

    canvas元素与img标签的不同之处在于，就像video，audio，或者 picture元素一样，很容易定义一些替代内容。由于某些较老的浏览器（尤其是 IE9 之前的 IE 浏览器）或者文本浏览器不支持 HTML 元素"canvas"，在这些浏览器上应该提示当前浏览器布置canvas api。

    不支持canvas的浏览器将会忽略canvas，并在其中渲染canvas中内容。而支持canvas的浏览器将会忽略在容器中包含的内容，并且只是正常渲染 canvas。

```html
  <canvas id="emerge" width="600" height="400" >
    当前浏览器不支持canvas，请下载最新浏览器
    <a href="https://www.google.cn/chrome/">下载Chrome</a>
  </canvas>
```

## 渲染上下文（The rendering context）

    `<canvas>` 元素创造了一个固定大小的画布，它公开了一个或多个**渲染上下文**，其可以用来绘制和处理要展示的内容。将会将注意力放在 2D 渲染上下文中。

    其他种类的上下文也许提供了不同种类的渲染方式；比如， WebGL 使用了基于OpenGL ES的 3D 上下文。 

2.CanvasRenderingContext2D

    可为 [`<canvas>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/canvas) 元素的绘图表面提供2D 渲染上下文。它用于绘制形状，文本，图像和其他对象。

```js
    // 1.找到canvas对象
    var c1 = document.getElementById("emerge");
    // 2.获取画笔，上下文对象
    var ctx = c1.getContext("2d");
```

兼容性检查

    替换内容是用于在不支持`<canvas>`标签的浏览器中展示的。通过简单的测试 getContext() 方法的存在，脚本可以检查编程支持性。上面的代码片段现在变成了这个样子：

```js
var c1 = document.getElementById('emerge');

if (c1.getContext){
  var ctx = canvas.getContext('2d');
  // drawing code here
} else {
  // canvas-unsupported code here
  c1.innerText = '当前浏览器不支持canvas，请下载最新浏览器'
}
```

一个简单例子

绘制了两个有趣的长方形，其中的一个有着 alpha 透明度。

```html
<html>
  <head>
    <script type="application/javascript">
      function draw() {
              // 1.找到canvas对象
              var c1 = document.getElementById("emerge");
              // 2.获取画笔，上下文对象
              var ctx = c1.getContext("2d");
              // 处理兼容问题，不支持canvas浏览器
              if(!ctx.getContext) c1.innerText = '当前浏览器不支持canvas，请下载最新浏览器'

              // 3.绘制图形
              // 3.1 绘制矩形 fillRect(x, y, width, height)
              ctx.fillStyle='rgba(218,255,0, 0.4)';
              ctx.fillRect(100, 150, 350, 150)

              ctx.fillStyle='rgba(0,0,255, 0.5)';
              ctx.fillRect(150, 200, 350, 150)
      }
    </script>
  </head>
  <body onload="draw();">
    <canvas id="canvas" width="150" height="150"></canvas>
  </body>
</html>
```

draw() 的函数，当页面加载结束的时候就会执行这个函数

## canvas绘制基本图形

接下来将学使用canvas进行绘制矩形，三角形，直线，圆弧和曲线，变得熟悉这些基本的形状。

<img src="./assets/1667121349188-0130b309-19cf-49db-a6bb-c02ea6c317ed.png" title="" alt="" data-align="center">

模板中有个宽 150px, 高 150px 的 canvas 元素。如上图所示，canvas 元素默认被网格所覆盖。通常来说网格中的一个单元相当于 canvas 元素中的一像素。栅格的起点为左上角（坐标为（0,0））。所有元素的位置都相对于原点定位。所以图中蓝色方形左上角的坐标为距离左边（X 轴）x 像素，距离上边（Y 轴）y 像素（坐标为（x,y））

## 绘制矩形

不同于 SVG，canvas 只支持两种形式的图形绘制：矩形和路径（由一系列点连成的线段）

canvas 提供了三种方法绘制矩形：

### 填充矩形 fillRect()

```js
ctx.fillRect(x, y, width, height);
```

`fillRect()`方法绘制一个填充了内容的矩形，这个矩形的开始点（左上点）在`(x, y)` ，它的宽度和高度分别由`width` 和 `height` 确定，填充样式由当前的`fillStyle` 决定

```html
<body>
  <div>
    <canvas id="cav" width="150" height="150" style="border: 1px dashed;">
    </canvas>
  </div>
  <script>
    // 1.找到canvas对象
    var cav = document.getElementById("cav")
    // 2.获取画布的 2D 渲染上下文
    var ctx2D = cav.getContext('2d'); 
    if (ctx2D.getContext) cav.innerText = '当前浏览器不支持canvas，请下载最新浏览器'

    // 3.绘制图形
    // 3.1 绘制矩形 fillRect(x, y, width, height) fillRect填充绘制
    // fillStyle 属性让长方形变成绿色
    // fillRect() 方法将它的左上角放在 (x:50, y:50)，把它的大小设置成宽 50 高 50
    ctx2D.fillStyle='rgba(67,142,219, 1)';
    ctx2D.fillRect(50, 50, 50, 50)

    // 拆分写法
    ctx2D.fillStyle='rgba(255,0,0, 1)';
    ctx2D.rect(100, 100, 50, 50)
    ctx2D.fill()
  </script>
</body>
```

![](./assets/iShot_2023-09-18_03.36.22.png)

### 描边矩形 strokeRect()

```js
ctx.strokeRect(x, y, width, height);
```

方法绘制一个描边矩形，其起点为`(x, y)` ，其大小由宽度和高度指定。

```html
  <div>
    <canvas id="cav" width="150" height="150" style="border: 1px dashed;">
    </canvas>
  </div>
  <script>
    // 1.找到canvas对象
    var cav = document.getElementById("cav")
    // 2.获取画布的 2D 渲染上下文
    var ctx2D = cav.getContext('2d'); 
    if (ctx2D.getContext) cav.innerText = '当前浏览器不支持canvas，请下载最新浏览器'

    // 3.绘制图形 fill英文单词是满足 填充
    // 3.1 绘制矩形 fillRect(x, y, width, height) fillRect填充模糊
    // fillStyle 属性让长方形变成绿色
    // fillRect() 方法将它的左上角放在 (x:50, y:50)，把它的大小设置成宽 50 高 50
    ctx2D.fillStyle='rgba(67,142,219, 1)';
    ctx2D.fillRect(50, 50, 50, 50)

    // 拆分写法
    ctx2D.strokeStyle='rgba(255,0,0, 1)';
    ctx2D.rect(100, 100, 50, 50)
    ctx2D.stroke()
  </script>
```

![](./assets/iShot_2023-09-18_03.36.35.png)

### 清除绘制矩形 clearRect()

`clearRect()` 方法在一个矩形区域内设置所有像素都是透明的 (`rgba(0,0,0,0)`)。这个矩形范围的左上角在 `(x, y)`，宽度和高度分别由 `width` 和`height`确定。

从左上角某个坐标开始清除比如：(60, 60 )

```html
<body>
  <div>
    <canvas id="cav" width="150" height="150" style="border: 1px dashed;">
    </canvas>
  </div>
  <script>
    // 1.找到canvas对象
    var cav = document.getElementById("cav")
    // 2.获取画布的 2D 渲染上下文
    var ctx2D = cav.getContext('2d'); 
    if (ctx2D.getContext) cav.innerText = '当前浏览器不支持canvas，请下载最新浏览器'

    // 3.绘制图形 
    // 3.1 绘制矩形 fillRect(x, y, width, height) fillRect路径绘制
    ctx2D.strokeStyle = "green";
    ctx2D.strokeRect(50, 50, 50, 50)

    // 再次绘制一个矩形
    ctx2D.fillStyle='red';
    ctx2D.fillRect(100,100, 50,50);

    // 清除画布上绘制的矩形
    // 从左上角清除，清除宽度和 高度 与canvas画布一样大小
    // ctx2D.clearRect(0, 0, cav.width, cav.height);
    // ctx2D.clearRect(60, 60, 50, 50);
    let height = 0
    let clearTime = setInterval(() => {
      if(height >= 50) clearInterval(clearTime)
      height++
      ctx2D.clearRect(60, 60, height, height);
    },100)

  </script>
</body>
```

![](./assets/2023-09-18%2003.38.58.gif)

### 分段绘制图形 beginPath 和 closePath

当画布上有两个不同的图形，一个图形是描边绘制，另一种是填充绘制，就需要分段绘制图形beginPath 和 closePath

```html
<body>
  <div>
    <canvas id="cav" width="150" height="150" style="border: 1px dashed;">
    </canvas>
  </div>
  <script>
    // 1.找到canvas对象
    var cav = document.getElementById("cav")
    // 2.获取画布的 2D 渲染上下文
    var ctx2D = cav.getContext('2d'); 
    if (ctx2D.getContext) cav.innerText = '当前浏览器不支持canvas，请下载最新浏览器'

    // 3.绘制图形 
    // 3.1 绘制矩形 fillRect(x, y, width, height) fillRect路径绘制
    // 分段绘制图形beginPath 和 closePath
    ctx2D.beginPath();
    ctx2D.strokeStyle = "green";
    // ctx2D.strokeRect(50, 50, 50, 50)
    // 拆分写法
    ctx2D.rect(50, 50, 50, 50)
    ctx2D.stroke() // 显示路径
    ctx2D.closePath();

    // 再次绘制一个矩形
    ctx2D.beginPath();
    ctx2D.fillStyle='red';
    // ctx2D.fillRect(100,100, 50,50);
    ctx2D.rect(100,100, 50,50);
    ctx2D.fill();
    ctx2D.closePath();

  </script>
</body>
```

## 绘制圆形

圆弧路径的圆心在 *(x, y)* 位置，半径为 *r*，根据*anticlockwise* （默认为顺时针）指定的方向从 *startAngle* 开始绘制，到 *endAngle* 结束。

```js
ctx.arc(x轴, y轴, radius半径, startAngle开始角度, endAngle结束角度, anticlockwise：是否顺时针);
```

```html
<body>
  <canvas id="cav" width="600" height="400" >
    当前浏览器不支持canvas，请下载最新浏览器
    <a href="https://www.google.cn/chrome/">下载Chrome</a>
  </canvas>

  <script>
    // 1.找到canvas对象
    var cav = document.getElementById("cav")
    // 2.获取画布的 2D 渲染上下文
    var ctx2D = cav.getContext('2d'); 
    if (!ctx2D.getContext) cav.innerText = '当前浏览器不支持canvas，请下载最新浏览器'

    // arc绘制圆弧
    // ctx.arc(圆心x,圆心y,半径, 开始角度, 结束角度, 顺时针逆时针)
    ctx2D.beginPath();
    // 0 到 360°
    ctx2D.arc(200, 200, 50, 0, 2 * Math.PI, true);
    ctx2D.fill(); // 填充画圆
    ctx2D.closePath();

    ctx2D.beginPath();
    ctx2D.strokeStyle='green';
    // 0 到 90°
    ctx2D.arc(300, 300, 50, 0,  Math.PI/2, false);
    ctx2D.stroke(); // 描边画圆
    ctx2D.closePath();
  </script>
</body>
```

![](assets/iShot_2023-09-23_01.49.58.png)

## 绘制路径

图形的基本元素是路径。路径是通过不同颜色和宽度的线段或曲线相连形成的不同形状

1. 首先，你需要创建路径起始点。
2. 然后你使用画图命令去画出路径。
3. 之后你把路径封闭。
4. 一旦路径生成，你就能通过描边或填充路径区域来渲染图形。

beginPath()：新建一条路径，生成之后，图形绘制命令被指向到路径上生成路径。

closePath()：闭合路径之后图形绘制命令又重新指向到上下文中。

stroke()：通过线条来绘制图形轮廓。

fill()：通过填充路径的内容区域生成实心的图形。

moveTo()：新建一个起点，起始点移动到 (x，y) 坐标的方法。

## 绘制直线

`lineTo` 方法画一条直线，使用 [`beginPath()`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/beginPath "beginPath()") 绘制路径的起始点，使用 `moveTo()`移动画笔，使用 [`stroke()`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/stroke "stroke()") 方法真正地画线。

```js
const canvas = document.getElementById("canvas");
const ctx = canvas.getContext("2d");

ctx.beginPath(); // Start a new path
ctx.moveTo(30, 50); // Move the pen to (30, 50)
ctx.lineTo(150, 100); // Draw a line to (150, 100)
ctx.stroke(); // Render the path
```

## 绘制三角

```html
    <script>
      // 1.找到canvas对象
      var cav = document.getElementById("cav");
      // 2.获取画布的 2D 渲染上下文
      var ctx2D = cav.getContext("2d");
      if (!ctx2D.getContext) cav.innerText = "当前浏览器不支持canvas，请下载最新浏览器";

      // 通过moveTo和lineTo 绘制直线
      ctx2D.beginPath();
      ctx2D.moveTo(100, 20);
      ctx2D.lineTo(10, 20);
      ctx2D.stroke();
      ctx2D.closePath();

      ctx2D.beginPath();
      ctx2D.setLineDash([5, 15, 10]); // 设置虚线
      ctx2D.moveTo(100, 30);
      ctx2D.lineTo(20, 30);
      ctx2D.stroke();
      ctx2D.closePath();

      // 通过moveTo和lineTo绘制三角形
      ctx2D.beginPath();
      ctx2D.setLineDash([]);
      ctx2D.moveTo(120, 50); // 属于起点 (x:120, y:50)
      ctx2D.lineTo(50, 120); // 起点到 （x:50, y:120)
      ctx2D.lineTo(50, 50);
      ctx2D.lineTo(120, 50);
      ctx2D.stroke();
      ctx2D.closePath();

      ctx2D.beginPath();
      ctx2D.moveTo(130, 60); // 属于起点 (x:120, y:50)
      ctx2D.lineTo(130, 130); // 起点到 （x:50, y:120)
      ctx2D.lineTo(60, 130);
      ctx2D.lineTo(130, 60);

      ctx2D.fill();
      ctx2D.closePath();
    </script>
```

<img src="assets/iShot_2023-09-23_01.53.23.png" title="" alt="" data-align="center">

## 移动笔触

一个非常有用的函数，而这个函数实际上并不能画出任何东西，也是上面所描述的路径列表的一部分。通常就是moveTo() 将笔触移动到指定的坐标 x 以及 y 上。

可以想象一下在纸上作业，一支钢笔或者铅笔的笔尖从一个点到另一个点的移动过程。

下面是绘制一个笑脸示例：

```html
  <canvas id="cav" width="600" height="400" >
    当前浏览器不支持canvas，请下载最新浏览器
    <a href="https://www.google.cn/chrome/">下载Chrome</a>
  </canvas>

  <script>
    // 1.找到canvas对象
    var cav = document.getElementById("cav")
    // 2.获取画布的 2D 渲染上下文
    var ctx2D = cav.getContext('2d'); 
    if (!ctx2D.getContext) cav.innerText = '当前浏览器不支持canvas，请下载最新浏览器'

    ctx2D.beginPath();
    ctx2D.strokeStyle='green';
    // 0 到 90°
    ctx2D.arc(300, 200, 100, 0,  2*Math.PI, false); // 脸

    ctx2D.moveTo(280, 175); // 计算方式，移动到距离中心点 (下一次要画圆中心点+画圆的半径)(260 + 20, 175)
    ctx2D.arc(260, 175, 20, 0, Math.PI*2, false); // 左眼睛

    ctx2D.moveTo(360, 175);
    ctx2D.arc(340, 175, 20, 0, Math.PI*2, false); // 右眼睛

    ctx2D.moveTo(375, 200);
    ctx2D.arc(300, 200, 75, 0, Math.PI, false); // 嘴巴眼睛
    ctx2D.stroke(); // 描边画圆
    ctx2D.closePath();
  </script>
```

![](./assets/iShot_2023-09-18_23.21.29.png)

## 绘制圆弧

基础点是蓝色的，两个控制点是红色的。

根据当前描点与给定的控制点 1 连接的直线，和控制点 1 与控制点 2 连接的直线，作为使用指定半径的圆的**切线**，画出两条切线之间的弧线路径。

```js
ctx.arcTo(x1, y1, x2, y2, radius);
```

![](assets/iShot_2023-09-22_17.07.21.png)

下面简单的代码片段

```html
  <canvas id="cav" width="600" height="400" >
    当前浏览器不支持canvas，请下载最新浏览器
    <a href="https://www.google.cn/chrome/">下载Chrome</a>
  </canvas>
  <script>
    // 1.找到canvas对象
    var cav = document.getElementById("cav")
    // 2.获取画布的 2D 渲染上下文
    var ctx2D = cav.getContext('2d'); 
    if (!ctx2D.getContext) cav.innerText = '当前浏览器不支持canvas，请下载最新浏览器'
    
    ctx2D.setLineDash([]);
    // arcTo(x1,y1,x2,y2,r)绘制圆弧arcTo(x坐标1,y坐标1, x坐标2,y坐标2, 半径)
    ctx2D.beginPath();
    // 先有设置起始点为
    ctx2D.moveTo(300, 100); // 第一个点，即描点
    ctx2D.arcTo(300, 200, 200, 100, 40); // 圆弧，通过三个点，来确定圆弧 弧度
    ctx2D.stroke();
    ctx2D.closePath();

    // 先有设置起始点为
    ctx2D.fillStyle='rgba(255,0,0,0.8)';
    ctx2D.fillRect(300, 100, 10, 10);

    ctx2D.fillStyle='rgba(255,255,0,0.8)';
    ctx2D.fillRect(300, 200, 10, 10);

    ctx2D.fillRect(200, 100, 10, 10);

    // 画虚线setLineDash([]); 传入一个数组
    ctx2D.setLineDash([5,5]);
    ctx2D.beginPath();
    ctx2D.moveTo(300, 100); // 第一个点
    ctx2D.lineTo(300, 200,);
    ctx2D.lineTo(200, 100);
    ctx2D.stroke();
    ctx2D.closePath();
    
    ctx2D.beginPath();
    ctx2D.arc(260, 103, 40, 0, 2 * Math.PI);
    ctx2D.stroke();
    ctx2D.closePath();
  </script>
```

## 绘制贝塞尔曲线

贝塞尔曲线(Bézier curve)，又称贝兹曲线或贝济埃曲线，是应用于二维图形应用程序的数学曲线。

一般的矢量图形软件通过它来精确画出曲线，贝兹曲线由线段与节点组成，节点是可拖动的支点，线段像可伸缩的皮筋，我们在绘图工具上看到的钢笔工具就是来做这种矢量曲线的。

比如项ps中的钢笔工具

### 一次贝塞尔曲线(线性贝塞尔曲线)

​ 一次贝塞尔曲线其实是一条直线。

![](assets/40655077.jpg)

### 二次贝塞尔曲线

```js
ctx.moveTo(x1, y1); // 启点坐标
quadraticCurveTo(cp1x, cp1y, x2, y2) // 控制点坐标
```

绘制二次贝塞尔曲线，cp1x,cp1y 为一个控制点，x,y 为结束点。

![](assets/1667150886162-f5d91ebd-e23a-4468-9749-f20c62dbd7d1.gif)

P0 到 P1 运行运动轨迹点（时间0.10到1.00），与P1到P2运行运动轨迹点，两点这相连的直线形成切线，切点形成曲线

![](assets/35792129.jpg)

```html
  <script>
    // 1.找到canvas对象
    var cav = document.getElementById("cav")
    // 2.获取画布的 2D 渲染上下文
    var ctx2D = cav.getContext('2d'); 
    if (!ctx2D.getContext) cav.innerText = '当前浏览器不支持canvas，请下载最新浏览器'

    // 绘制贝塞尔曲线
    // ctx.quadraticCurveTo(控制x,控制y,半径, 结束点x，结束点y)
    ctx2D.beginPath();
    ctx2D.moveTo(200, 150); // 起始点
    ctx2D.quadraticCurveTo(250, 50, 350, 150);
    ctx2D.strokeStyle='red';
    
    ctx2D.stroke();
    ctx2D.closePath();

    ctx2D.beginPath();
    ctx2D.fillStyle='rgba(255, 255, 0, 0.8)';
    ctx2D.arc(200, 150, 5, 0, Math.PI * 2); // 起始点

    ctx2D.moveTo(350, 150);
    ctx2D.arc(350, 150, 5, 0, Math.PI * 2); // 终点
    ctx2D.fill();
    ctx2D.closePath();

    // 控制点
    ctx2D.beginPath();
    ctx2D.arc(250, 50, 5, 0, Math.PI * 2);
    ctx2D.fillStyle='rgba(255, 0, 0, 0.8)';
    ctx2D.fill();
    ctx2D.closePath();

    // 三个点连线
    ctx2D.beginPath();
    ctx2D.setLineDash([3,3]);
    ctx2D.strokeStyle='gray';
    ctx2D.moveTo(200, 150); 
    ctx2D.lineTo(250, 50);
    ctx2D.lineTo(350, 150);
    ctx2D.stroke();
    ctx2D.closePath();

    // 切线
    ctx2D.beginPath();
    ctx2D.setLineDash([]);
    ctx2D.strokeStyle='green';
    ctx2D.moveTo(215, 120); 
    ctx2D.lineTo(280, 80);
    ctx2D.stroke();
  </script>
```

<img src="assets/iShot_2023-09-22_23.47.40.png" title="" alt="" data-align="center">

使用多个贝塞尔曲线来渲染对话气泡

```html
  <script>
    // 1.找到canvas对象
    var cav = document.getElementById("cav")
    // 2.获取画布的 2D 渲染上下文
    var ctx2D = cav.getContext('2d'); 
    if (!ctx2D.getContext) cav.innerText = '当前浏览器不支持canvas，请下载最新浏览器'

    // 绘制贝塞尔曲线
    // ctx.quadraticCurveTo(控制x,控制y,半径, 结束点x，结束点y)
    ctx2D.beginPath();
    ctx2D.moveTo(200, 150); // 起始点
    ctx2D.quadraticCurveTo(210, 100, 280, 100);
    ctx2D.quadraticCurveTo(350, 100, 360, 150);
    ctx2D.quadraticCurveTo(360, 200, 300, 200);
    ctx2D.quadraticCurveTo(280, 230, 250, 230);
    ctx2D.quadraticCurveTo(260, 220, 260, 200);
    ctx2D.quadraticCurveTo(200, 200, 200, 150);
    ctx2D.strokeStyle='red';
    
    ctx2D.stroke();
    ctx2D.closePath();
  </script>
```



### 三次贝塞尔曲线

```js
ctx.moveTo(x1, y1); // 启点坐标
bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y) // 控制点坐标cp1、控制点2坐标 cp2
```

绘制三次贝塞尔曲线，cp1x,cp1y为控制点一，cp2x,cp2y为控制点二，x,y为结束点。

![](assets/657150992425-4cd7f759-76d7-466d-836a-1be44e801f83.gif)

P0 到 P1 运行运动轨迹点，与P1 到 P2运行运动轨迹点，两点之间连线，绿色Q0 与 Q1

P1 到 P2 运行运动轨迹点，与P2 到 P3运行运动轨迹点，两点之间连线，绿色Q1 与 Q2

Q0 与 Q1 之间的连线，Q1 与 Q2之间的连线，形成R0 与 R1

![](assets/55999388.jpg)

上图的图能够很好的描述两者的关系，二次贝塞尔曲线有一个开始点（蓝色）、一个结束点（蓝色）以及一个控制点（红色），而三次贝塞尔曲线有两个控制点。

```html
  <script>
    // 1.找到canvas对象
    var cav = document.getElementById("cav")
    // 2.获取画布的 2D 渲染上下文
    var ctx2D = cav.getContext('2d'); 
    if (!ctx2D.getContext) cav.innerText = '当前浏览器不支持canvas，请下载最新浏览器'

    // 绘制贝塞尔曲线
    // ctx.quadraticCurveTo(控制x,控制y,半径, 结束点x，结束点y)
    ctx2D.beginPath();
    ctx2D.moveTo(200, 150); // 起始点
    
    ctx2D.bezierCurveTo(350, 50, 470, 70, 450, 150);
    ctx2D.strokeStyle='red';
    
    ctx2D.stroke();
    ctx2D.closePath();

    ctx2D.beginPath();
    ctx2D.fillStyle='rgba(255, 255, 0, 0.8)';
    ctx2D.arc(200, 150, 5, 0, Math.PI * 2); // 起始点

    ctx2D.moveTo(350, 150);
    ctx2D.arc(450, 150, 5, 0, Math.PI * 2); // 终点
    ctx2D.fill();
    ctx2D.closePath();

    // 控制点
    ctx2D.beginPath();
    ctx2D.arc(350, 50, 5, 0, Math.PI * 2);
    ctx2D.moveTo(350, 80);
    ctx2D.arc(470, 70, 5, 0, Math.PI * 2);
    ctx2D.fillStyle='rgba(255, 0, 0, 0.8)';
    ctx2D.fill();
    ctx2D.closePath();

    // 三个点连线
    ctx2D.beginPath();
    ctx2D.setLineDash([3,3]);
    ctx2D.strokeStyle='gray';
    ctx2D.moveTo(200, 150); 
    ctx2D.lineTo(350, 50);
    ctx2D.lineTo(470, 70);
    ctx2D.lineTo(450, 150);
    ctx2D.stroke();
    ctx2D.closePath();

    // 切线
    ctx2D.beginPath();
    ctx2D.setLineDash([]);
    ctx2D.strokeStyle='green';
    ctx2D.moveTo(275, 100); 
    ctx2D.lineTo(420, 60);
    ctx2D.lineTo(465, 100);
    ctx2D.stroke();

    ctx2D.beginPath();
    ctx2D.strokeStyle='blue';
    ctx2D.moveTo(360, 76); 
    ctx2D.lineTo(460, 95);
    ctx2D.stroke();
  </script>
```

<img src="assets/iShot_2023-09-23_01.43.57.png" title="" alt="" data-align="center">

三次贝塞尔曲线画爱心

```html
    <script>
      // 1.找到canvas对象
      var cav = document.getElementById("cav");
      // 2.获取画布的 2D 渲染上下文
      var ctx2D = cav.getContext("2d");
      if (!ctx2D.getContext)
        cav.innerText = "当前浏览器不支持canvas，请下载最新浏览器";

      // 绘制贝塞尔曲线
      // ctx.bezierCurveTo(控制x,控制y,控制x,控制y, 结束点x，结束点y)
      ctx2D.moveTo(220, 100);
      ctx2D.bezierCurveTo(100, 50, 80, 170, 220, 250);
      ctx2D.bezierCurveTo(360, 170, 340, 50, 220, 100);
      ctx2D.strokeStyle = "red";

      ctx2D.stroke();
      ctx2D.closePath();

      ctx2D.beginPath();
      ctx2D.fillStyle = "rgba(255, 255, 0, 0.8)";
      ctx2D.arc(100, 50, 5, 0, Math.PI * 2); // 起始点

      ctx2D.moveTo(90, 170);
      ctx2D.arc(80, 170, 5, 0, Math.PI * 2); // 终点

      ctx2D.moveTo(390, 170);
      ctx2D.arc(360, 170, 5, 0, Math.PI * 2); // 终点

      ctx2D.moveTo(330, 50);
      ctx2D.arc(340, 50, 5, 0, Math.PI * 2); // 终点
      ctx2D.fill();
      ctx2D.closePath();
    </script>
```

![](assets/iShot_2023-09-23_02.43.03.png)

## Path2D 对象

    在前面例子中看到的，使用一系列的路径和绘画命令来把对象“画”在画布上。为了简化代码和提高性能，Path2D对象已可以在较新版本的浏览器中使用，用来缓存或记录绘画命令，这样你将能快速地回顾路径。

    **`Path2D()`** 构造函数返回一个新的 `Path2D` 对象的实例，可以选择另一条路径作为参数（创建一个拷贝），或者选择 [SVG path](https://developer.mozilla.org/zh-CN/docs/Web/SVG/Tutorial/Paths) 数据构成的字符串。

    所有的路径方法比如moveTo, rect, arc或quadraticCurveTo等，如我们前面见过的，都可以在 Path2D 中使用。

    Path2D API 添加了 addPath作为将path结合起来的方法。当你想要从几个元素中来创建对象时，这将会很实用。比如：

`Path2D.addPath(path [, transform])`

添加了一条路径到当前路径（可能添加了一个变换矩阵）。

```js
const canvas = document.getElementById("canvas");
const ctx = canvas.getContext("2d");

let path1 = new Path2D();
path1.rect(10, 10, 100, 100);

let path2 = new Path2D(path1);
path2.moveTo(220, 60);
path2.arc(170, 60, 50, 0, 2 * Math.PI);

ctx.strokeStyle = "red";
ctx.stroke(path2);
```

```html
    <canvas id="cav" width="600" height="400">
      当前浏览器不支持canvas，请下载最新浏览器
      <a href="https://www.google.cn/chrome/">下载Chrome</a>
    </canvas>

    <script>
      // 1.找到canvas对象
      var cav = document.getElementById("cav");
      // 2.获取画布的 2D 渲染上下文
      var ctx2D = cav.getContext("2d");
      if (!ctx2D.getContext)
        cav.innerText = "当前浏览器不支持canvas，请下载最新浏览器";

      // 有了 Path2D 来封装路径，专门用于绘制路径
      var path2D = new Path2D();

      path2D.moveTo(220, 100);
      path2D.bezierCurveTo(100, 50, 80, 170, 220, 250);
      path2D.bezierCurveTo(360, 170, 340, 50, 220, 100);
      ctx2D.strokeStyle = "red";
      ctx2D.stroke(path2D);

      var chat2D = new Path2D();
      chat2D.moveTo(200, 150); // 起始点
      chat2D.quadraticCurveTo(210, 100, 280, 100);
      chat2D.quadraticCurveTo(350, 100, 360, 150);
      chat2D.quadraticCurveTo(360, 200, 300, 200);
      chat2D.quadraticCurveTo(280, 230, 250, 230);
      chat2D.quadraticCurveTo(260, 220, 260, 200);
      chat2D.quadraticCurveTo(200, 200, 200, 150);
      ctx2D.fillStyle='rgba(0, 0, 255, 0.5)';
      ctx2D.strokeStyle='rgba(255, 255, 0, 1)';
      ctx2D.stroke(chat2D);
      ctx2D.fill(chat2D);
    </script>
```

![](assets/iShot_2023-09-23_04.04.35.png)

Path2D 选择 [SVG path](https://developer.mozilla.org/zh-CN/docs/Web/SVG/Tutorial/Paths) 数据构成的字符串

```js
      // M10 10移动到moveTo(x,y) h水平移到80位置 v垂直移动到80 h水平移到-80 z回到启点
      var svg2D = new Path2D('M10 10 h 80 v 80 h -80 z'); 
      ctx2D.stroke(svg2D)
```
