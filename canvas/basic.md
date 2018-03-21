## 初识canvas

canvas现在越来越受前端开发者的青睐，除了能帮助开发者提升性能，优化前端问题的解决方案外，还可以做视音频的处理，和游戏制作等。现在有很多优秀的开源项目如echart，D3等都引入了canvas。

### 零基础canvas

想要学习好canvas，其实很简单，就是掌握它的基本语法，了解它的局限性，熟悉它的使用场景，结合更多的案例练手。

#### 浏览器支持

|Chrome|Safari|Firefox|IE|Opear|iOS Safari|Android Brower|
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|4.0|3.2|2.0|9.0|9.0|3.2|2.1|

#### 基本语法

canvas作为HTML5的新增组件，中文又被称为画布，意思就是开发者可以在该对象上进行写写画画，绘制各种图表，添加动画等。因此canvas其实是一个绘画的容器，该容器包含各种绘画工具（canvas API），在HTML5之前我们使用的是flash。

第一步，创建一个可以提供绘画的容器，然后指定容器的大小。

```
// 在支持html5语法的文件中加入下面的元素标签即可
<canvas id="canvas" width="750" height="600"></canvas>
```

当然，我们也可以使用js来操作dom，动态的添加画布。

我们知道支持canvas的浏览器有很多，那么如何写一个更友好的代码，成了我们需要关注的一点。通常我们有两种解决方案。

```
// 第一种直接在canvas种加入提示语
<canvas id="canvas" width="750" height="600">
	<p>你的浏览器不支持Canvas，请下载最新版本!</p>
</canvas>
// 嗅探
var canvas = document.getElementById('canvas');
if (canvas.getContext) {
    console.log('你的浏览器支持Canvas.');
} else {
    console.log('你的浏览器不支持Canvas，请下载最新版本!');
}

```

第二步，使用canvas API获得容器对象，指定是绘制3D，还是2D图形。

> 接下来所有实例无特殊说明均为2D

```
// 使用canvas.getContext('2d')方法，通过传入指定的参数'2d'，
// 可以返回一个绘制对象（CanvasRenderingContext2D），
// 所有的绘画操作都要使用它。
var ctx = canvas.getContext('2d')
// 3D操作

var ctx = canvas.getContext('webgl')
```

第三步，图形的组成元素（点，线，面，图形，文本，样式(颜色，大小)）

[参考API](http://www.w3school.com.cn/tags/html_ref_canvas.asp)

1. 路径

```
// 获得渲染的2d对象
var ctx = document.getElementById('canvas').getContext('2d');

// 开始绘画第一条路径
ctx.beginPath()
// 绘制起始点,默认为(0,0)，可以使用moveTo()，
// 改变起始点
ctx.moveTo(10, 0)
// 创建终点
ctx.lineTo(10, 200)

// 绘制已定义的路径
ctx.stroke()

// 绘制第二条路经
ctx.beginPath()
ctx.moveTo(0, 10)
ctx.lineTo(200, 10)
ctx.stroke()

```

其实，对于上面定义一条路径绘制一条的使用方式不是很友好，可以将要最终绘制的图形路径都定义完毕，然后统一绘制。

```
// 获得渲染的2d对象
var ctx = document.getElementById('canvas').getContext('2d');

// c创建路径对象
var path = new Path2D()
// 开始绘画第一条路径
path.moveTo(10, 0)
// 创建终点
path.lineTo(10, 200)

// 绘制第二条路经
path.moveTo(0, 10)
path.lineTo(200, 10)

// 绘制已定义的路径
ctx.stroke(path)
```

2. 图形

```
var ctx = document.getElementById('canvas').getContext('2d');

// 绘制矩形
ctx.beginPath()
ctx.rect(10, 10, 300, 150)
ctx.stroke()
// 绘制圆形
ctx.beginPath()
ctx.arc(160, 85, 50, 0, 2*Math.PI)
ctx.stroke()
// 绘制椭圆形
var path = new Path2D
path.ellipse(160, 85, 50, 20, 0, 0, 2*Math.PI)
ctx.stroke(path)
```

上面是我们可以直接使用API进行绘制的，要想绘制自定义的图形我们就需要使用路径。

```
// 自定义图形
var path = new Path2D()

// 三角形
path.moveTo(10, 10)
path.lineTo(10, 50)
path.moveTo(10, 50)
path.lineTo(100, 50)
path.moveTo(100, 50)
path.lineTo(10, 10)

// 圆角矩形
path.moveTo(10, 20)
path.lineTo(10, 90)

// left top
path.arc(20, 20, 10, Math.PI, 3*Math.PI/2, false)
path.moveTo(20, 100)
path.lineTo(140, 100)

// left bottom
path.arc(20, 90, 10, Math.PI/2, Math.PI, false)
path.moveTo(150, 90)
path.lineTo(150, 20)

// right bottom
path.arc(140, 90, 10, 0, Math.PI/2, false)

path.moveTo(140, 10)
path.lineTo(20, 10)

// right top
path.arc(140, 20, 10, 3*Math.PI/2, 2*Math.PI, false)

ctx.stroke(path)
```

3. 文本

```
var ctx = document.getElementById('canvas').getContext('2d');

ctx.font = "30px Microsoft yahei"
ctx.fillText('canvas font fillText', 10, 30)
// 将字体渲染为路径
ctx.strokeText('canvas font strokeText', 10, 80)
```

4. 图像

```
var ctx = document.getElementById('canvas').getContext('2d');

var img = new Image()
img.src = "https://avatars3.githubusercontent.com/u/19884132?s=400&u=726a55af181fa2f01d54cdf0be894e68e546c568&v=4"

img.onload = function() {
	ctx.dreawImage(img, 100, 100)
}
```

5. 样式

```
var ctx = document.getElementById('canvas').getContext('2d');
ctx.fillStyle() // 填充的颜色/渐变/对象
ctx.strokeStyle() // 笔触的颜色/渐变/对象
ctx.shadowColor() // 阴影的颜色
ctx.shadowBlur() // 阴影的模糊程度
ctx.shadowOffsetX() // 阴影的水平偏移
ctx.shadowOffsetY() // 阴影的垂直偏移
ctx.createLinearGradient() // 线性渐变
var grd = ctx.createRadioGradient() // 放射渐变
grd.addColorStop() // 渐变对象的颜色和停止位置
var pat = ctx.createPattern(img, 'repeat') // 在指定的方向上重复指定的元素
```

### 归纳总结

只需要四步即可完成一个画布：

1.布置画布：在html文档中添加<canvas></canvas>元素标签

2.获取画布：通过获取<canvas id="canvas"></canvas>的id选择器，来获取canvas对象

3.获得画笔：通过向Canvas.getContext()传入参数'2d'/'wegl'，来获得相应的绘画环境即可

4.自定义内容：路径/图形/图像/样式来渲染生成所需的图形


