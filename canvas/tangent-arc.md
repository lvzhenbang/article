## 利用切线画弧

这里我们将要使用一个方法CanvasRenderingContext2D.arcTo()，它一共有五个参数，这五个前两个表示一个控制点，它是该弧线两个端点的切线的焦点；第三四个参数表示一个控制点这个控制点是一个切点，该切点表示弧线绘制方向的最后一个点；最后一个参数表示弧线的半径。

下面我们绘制一个椭圆矩形，以前我们在[初识canvas中的图形-自定义图形](https://github.com/lvzhenbang/article/blob/master/canvas/basic.md)中使用直线和1/4圆弧组成方式实现过，使用的方法是lineTo(), arc()这里我们使用arcTo()方法来实现。

![arTo示意图](https://github.com/lvzhenbang/article/blob/master/canvas/img/arcto.jpg)

### 代码实例

绘画的顺序为上->右->下->左

```
var ctx = document.getElemntById('canvas').getContext('2d')
// 上直线 右弧线
ctx.moveTo(30, 10)
ctx.arcTo(80, 10, 80, 30, 20)
// 右直线 下弧线
ctx.moveTo(80, 30)
ctx.arcTo(80, 80, 60, 80, 20)
// 下直线 左弧线 
ctx.moveTo(60, 80)
ctx.arcTo(10, 80, 10, 60, 20)
// 左直线 上弧线
ctx.moveTo(10, 60)
ctx.arcTo(10, 10, 30, 10, 20)
// 渲染路径
ctx.stroke()
```
