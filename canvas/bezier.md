## 贝塞尔曲线

贝塞尔曲线是应用于二维图形应用程序的数学曲线。

它由起始点、控制点、终点组成。通过调整控制点，贝塞尔曲线会随着变化。

如果有n-1个控制点，那么就叫做n阶贝塞尔曲线。

在canvas中提供了CanvasRenderingContext2D.quadracticCurveTo()创建二阶贝塞尔曲线，CanvasRenderingContext2D.bezierCurveTo()创建三阶贝塞尔曲线。

### canvas中的贝塞尔曲线

在canvas中使用moveTo()指定贝塞尔曲线的开始点，而quadraticCurveTo()和bezierCurveTo()中最后传入的两个参数表示贝塞尔曲线的终点，而其余的参数每两个连续的参数表示一个控制点。

1. 二次贝塞尔曲线

```
ctx.moveTo(10, 10)
ctx.quadraticCurveTo(20, 50, 50, 10)
ctx.stroke()

```

![示例图](https://github.com/lvzhenbang/article/blob/master/canvas/img/quadraticCurveTo.png)

2. 三次贝塞尔曲线

```
ctx.moveTo(10, 10)
ctx.bezierCurveTo(20, 50, 50, 50, 100, 10)
ctx.stroke()
```

![示例图](https://github.com/lvzhenbang/article/blob/master/canvas/img/bezierCurveTo.png)

### 实例代码

使用贝塞尔曲线画云

```
/**
 * [drawCloud 使用画笔画一片云]
 * @param  {[type]} ctx               [画笔]
 * @param  {[type]} cloudColor        [云颜色]
 * @param  {[type]} xPos              [起点x坐标]
 * @param  {[type]} yPos              [起点y坐标]
 * @param  {[type]} yPosMin           [终点y坐标上限]
 * @param  {[type]} yPosMax           [终点y坐标下限]
 * @param  {[type]} xPosIncrement     [终点x坐标增量]
 * @param  {[type]} yPosIncrement     [终点y坐标增量]
 * @param  {[type]} xPosChangeMin     [终点x坐标最小增量]
 * @param  {[type]} yControlIncrement [控制点y坐标增量]
 * @param  {[type]} yControlCchangeMin[控制点y坐标最小增量]
 * @return {[type]}                   []
 */
function drawCloud(ctx, cloudColor, xPos, yPos, yPosMin, yPosMax, xPosIncrement, yPosIncrement, xPosChangeMin, yControlIncrement, yControlChangeMin) {
	ctx.fillStyle = cloudColor
	ctx.beginPath()
	ctx.moveTo(xPos, yPos)
	var lastX

	while (xPos < canvas.width) {
	  	lastX = xPos
		xPos += Math.floor(Math.random() * xPosIncrement + xPosChangeMin)
		yPos += Math.floor(Math.random() * yPosIncrement - yPosIncrement / 2)

		while (yPos < yPosMin) {
			yPos += Math.floor(Math.random() * yPosIncrement / 2) 
		}

		while (yPos > yPosMax) {
			yPos -= Math.floor(Math.random() * yPosIncrement / 2) 
		}
		// 控制点
	  	controlX = (lastX + xPos) / 2

	  	controlY = yPos - Math.floor(Math.random() * yControlIncrement + yControlMin)
	  	// 画云的轮廓路径曲线
		ctx.quadraticCurveTo(controlX, controlY, xPos, yPos)
	}

	// 画一个闭合的云路径
	ctx.lineTo(canvas.width, yPosMax)
	ctx.lineTo(canvas.width, yPosMax)
	ctx.lineTo(0, yPosMax)

	ctx.fill()
}
// 天空
ctx.fillStyle = '#03a9f4'
ctx.fillRect(0, 0, canvas.width, 175)
// 云
drawCloud(ctx, '#fff', 0, 150, 125, 175, 20, 60, 125, 20, 40)


```