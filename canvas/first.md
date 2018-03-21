## 熟悉使用canvas绘画

上一篇归根到底主要是认识画布实现的四个步骤。这一篇主要介绍第四步自定义内容。

### 认识canvas提供的绘画方式

一种是填充内容,如样式fillStyle()，文字fillText()；
另一个就是描边，如stroke()，strokeStyle()

### canvas绘制内容

fill填充内容很简单直接填充即可。

描边需要先绘制路径，beginPath()开始一条路径，moveTo()路径的起点，lineTo()路径的暂停点，closePath()路径的终点，当然我们可以使用path2D来创建一个复杂的路径。

### canvas提供的笔刷功能

```
lineWidth 线条的宽度，
lineCap线条的结束端点样式(默认值为butt，round，square)，
lineJoin线条的交点的类型(默认值miter，bevel：对角线斜角，round：一个圆)，
miterLimit线条焦点的最大斜接长度，
strokeStyle 线条的颜色。
```

### 封装绘制方法

canvas的方法封装和一般的dom操作和函数方法封装方法一致。

1. 绘制矩形

```
function drawRect(ctx, x, y, width, height, fillColor, lineWidth, lineColor, fn) {
	ctx.beginPath()
	ctx.moveTo(x, y)
	ctx.lineTo(x + width, y)
	ctx.lineTo(x + width, y + height)
	ctx.lineTo(x, y + height)
	ctx.lineTo(x, y)
	ctx.closePath()

	ctx.lineWidth = lineWidth
	ctx.strokeStyle = lineColor
	ctx.fillStyle = fillColor

	ctx.fill()
	ctx.stroke()
}

drawRect(10, 10, 200, 300, '#ccc', '2px', '#ff0000')
```

2. 绘制渐变

```
function drawGradient(ctx, pos, startColor, endColor) {
	var grd = ctx.createLinearGradient(pos[0].x, pos[0].y, pos[1].x, pos[1].y)
	grd.addColorStop(0, startColor)
	grd.addColorStop(1, endColor)
	ctx.fillStyle = grd
}

drawGradient(ctx, [{x: 10, y: 10}, {x: 200, y: 10}], '#000', '#fff')

ctx.fillRect(10, 10, 200, 100)
```