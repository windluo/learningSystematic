## 第15章、使用 Canvas 绘图

本章内容

> 理解<canvas>元素
>
> 绘制简单的 2D 图形
>
> 使用 WebGL 绘制 3D 图形

`<canvas>` 元素由 HTML5 新增，在页面中设定一个区域，可以通过 JS 在其中绘制图形。该功能非常受欢迎。

`<canvas>` 能绘制 2D 图形，结合 `WebGl`进阶，可以绘制 3D 图形。

### 15.1 基本用法

要使用`<canvas>`元素，必选先设置 width 和 height 属性，指定可以绘图的区域大小。

```html
<canvas id="drawing" width=" 200" height="200">A drawing of something.</canvas>
```

`<canvas>`元素内的文本是提示给不支持该元素的浏览器的。

### 15.2 2D上下文

使用 2D 绘图上下文提供的方法，可以绘制简单的 2D 图形，比如矩形、弧形和路径。2D 上下文的坐标开始于`<canvas>`元素的左上角。

#### 15.2.1 填充和描边

利用 fillStyle 和 strokeStyle 属性给`<canvas>`元素填充和描边，就是设置颜色或背景，该值可以是字符串、渐变对象或模式对象。

```js
var context = $ele.getContext("2d");
context.strokeStyle = "red";
context.fillStyle = "#0000ff"; 
```

#### 15.2.2 绘制矩形

绘制矩形的方法：`fillRect()`、`strokeRect()`、`clearRect()`。这三个方法都接收四个参数：矩形 x 坐标、矩形 y 坐标、矩形宽度、矩形高度。

```js
var $ele = document.getElementById('can1');
var context = $ele.getContext('2d');

// 绘制两个不同颜色的填充色矩形
context.fillStyle = '#ff0000';
context.fillRect(10, 10, 50, 50);
context.fillStyle = 'rgba(0,0,255,0.5)';
context.fillRect(30, 30, 50, 50);

// 在相同的位置绘制两个同色的描边色矩形
context.strokeStyle = "green";
context.strokeRect(10, 10, 50, 50);
context.strokeRect(30, 30, 50, 50);

// 在矩形叠加处清除一小块
context.clearRect(40, 40, 10, 10); // 这个方法其实是把指定区域变透明
```

#### 15.2.3 绘制路径

2D 绘制上下文支持绘制各种复杂的形状和线条。

首先必须调用`beginPath()`方法，表示要开始绘制路径，然后结合`arc()`、`moveTo()`、`rect()`等方法绘制想要的图形。

绘图完成后可以调用`fill()`、`stroke()`填充或描边，调用`clip()`剪切区域。

```js
// 开始路径
context.beginPath();
// 绘制外圆
context.arc(100, 100, 99, 0, 2 * Math.PI, false);
// 绘制内圆
context.moveTo(194, 100);
context.arc(100, 100, 94, 0, 2 * Math.PI, false);
// 绘制分针
context.moveTo(100, 100);
context.lineTo(100, 15);
// 绘制时针
context.moveTo(100, 100);
context.lineTo(35, 100);
// 描边路径
context.stroke(); 
```

上面的代码绘制了一个计时表。

#### 15.2.4 绘制文本

绘制文本主要是用这两个方法：`fillText()` 和 `strokeText()`。

#### 15.2.5 变换

变换主要用到的是这些方法：`rotate(angle)`、`scale(scaleX, scaleY)`、`translate(x, y)`等等。

#### 15.2.6 绘制图像

需要用到`drawImage()`方法。这个方法用到的图像必须是同源的。

#### 15.2.7 阴影

2D 上下文根据这几个属性值设定阴影：`shadowColor`、`shadowOffsetX`、`shadowOffsetY`、`shadowBlur`。

```js
// 设置阴影
context.shadowOffsetX = 5; // 阴影的x轴方向的偏移量
context.shadowOffsetY = 5; // 阴影的y轴方向的偏移量
context.shadowBlur = 4; // 阴影的模糊像素数
context.shadowColor = "rgba(0, 0, 0, 0.5)";
// 绘制红色矩形
context.fillStyle = "#ff0000";
context.fillRect(10, 10, 50, 50);
// 绘制蓝色矩形
context.fillStyle = "rgba(0,0,255,1)"; 
context.fillRect(30, 30, 50, 50); 
```

#### 15.2.8 渐变

线性渐变用这个方法`createLinearGradient()`。这个方法接收 4 个参数：起点的 x 坐标、起点的 y 坐标、终点的 x 坐标、终点的 y 坐标。

```js
var gradient = context.createLinearGradient(30, 30, 70, 70);
gradient.addColorStop(0, "white");
gradient.addColorStop(1, "black");

// 绘制红色矩形
context.fillStyle = "#ff0000";
context.fillRect(10, 10, 50, 50);
// 绘制渐变矩形
context.fillStyle = gradient;
context.fillRect(30, 30, 50, 50);
```

径向渐变用这个方法：`createRadialGradient()`。这个方法接6个参数：起点圆的圆心坐标和半径、终点圆的圆心坐标和半径。

```js
var gradient = context.createRadialGradient(55, 55, 10, 55, 55, 30);
gradient.addColorStop(0, "white");
gradient.addColorStop(1, "black");
// 绘制红色矩形
context.fillStyle = "#ff0000";
context.fillRect(10, 10, 50, 50);
// 绘制渐变矩形
context.fillStyle = gradient;
context.fillRect(30, 30, 50, 50); 
```

- 线性渐变（Linear Gradients）- 向下/向上/向左/向右/对角方向，线性变化。
- 径向渐变（Radial Gradients）- 由它们的中心定义，径向变化，像波纹一样扩散。

#### 15.2.9 模式

使用`createPattern()`方法。其实就是重复的图像。

#### 15.2.10 使用图像数据

使用`getImageData()`获取原始图像的数据，可以实现例如灰阶过滤等诸多功能。

#### 15.2.11 合成

利用`globalAlpha`设置所有绘制的透明度，该值范围介于`0~1`。

利用`globalCompositionOperation`设置后绘制的图像怎样与先绘制的图像结合，比如让后绘制的图形居于上方。

```js
// 重置全局透明度为0.5，后面的图形的透明度都是0.5了
context.globalAlpha = 0.5;
// 绘制红色矩形
context.fillStyle = "#ff0000";
context.fillRect(10, 10, 50, 50);
// 设置合成操作，‘destination-over’让后绘制的图形居于先绘制的图形下方
// 这里不设置的话，蓝色是在红色之上的
context.globalCompositeOperation = "destination-over";
// 绘制蓝色矩形
context.fillStyle = "rgba(0,0,255,1)";
context.fillRect(30, 30, 50, 50);
```

### 15.3 WebGL

WebGL 是针对 Canvas 的 3D 上下文。

> 有非常成熟的插件库`three.js`，强烈推荐使用。



### 总结

Canvas 绘图和 WebGL 绘图都有成熟的插件库，推荐在了解这两块的基础上去使用成熟的产品。