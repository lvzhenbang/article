## canvas中的图形裁剪

canvas中的剪切是指绘制路径内部区域的图像，而非路径外部区域的图像。

提示：在初次接触clip()方法时，我们发现裁剪后画布会变小，而且随着裁剪次数的增加画布会越来越小。如果要保持画布裁剪后的大小我们应该使用save()保存裁剪前的画布状态，使用store()来恢复保存的画布状态。

### 示例代码

```
ctx.font = "48px serif"
// 方位角 文字
ctx.save()
ctx.textAlign = "start"
ctx.fillText("left top", 0, 50)
ctx.textAlign = "end"
ctx.fillText("right top", 750, 50)
ctx.textAlign = "start"
ctx.fillText("left bottom", 0, 600)

ctx.textAlign = "end"
ctx.fillText("right bottom", 750, 600)
ctx.restore()

// 以正方形为路径的剪切
ctx.save()

ctx.fillStyle = "#000"
ctx.beginPath()
ctx.rect(250, 175, 250, 250)
ctx.clip()

ctx.restore()

// 红色圆
ctx.beginPath()
ctx.strokeStyle = "#ccc"
ctx.lineWidth = 3
ctx.arc(375, 300, 130, 0, Math.PI*2, false)
ctx.stroke()
ctx.closePath()

// 居中显示的路径字体
ctx.font = "16px Microsoft yahei"
ctx.textAlign = "center"
ctx.textBaseline = "middle"
ctx.fillText('wo wa...', 375, 300)
```

![未保存画布状态的效果图](https://github.com/lvzhenbang/article/blob/master/canvas/img/n-c.png)

![保存画布状态并在裁剪后恢复画布状态的效果图](https://github.com/lvzhenbang/article/blob/master/canvas/img/n-c.png)

一般情况我们使用clip()后会缩小绘画显示的区域，但是这不影响原来的绘画内容显示。如果我们想要在裁剪后使用原来的绘画区域，我们要在裁剪前使用save()保存裁剪前的绘画状态，然后在裁剪后恢复之前保存的绘画状态即可。就像上例中以正方形路径内部为裁剪区域的实例代码一样。