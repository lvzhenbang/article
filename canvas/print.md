## canvas导出为图片并下载

在前端我们下载资源是通过<a></a>元素标签，将所需下载的资源的地址复制给该元素的href属性，然后设置该元素的属性download（该属性可指定下载资源的名称）。

目前只有支持HTML5 的 <a></a>元素标签属性download的浏览器，可实现下载功能。

canvas的API提供了toDataURL()方法将画布内容转化为[Data URI](https://baike.baidu.com/item/Data%20URI/16297127?fr=aladdin)，然后将转化结果赋值给<a></a>元素标签的属性href即可，最后在浏览器中单击该链接就可以实现下载。

### 常见的canvas导出形式

打印canvas分为canvas元素的打印、将dom元素绘制canvas上打印。

下载canvas的步骤：

* 1.将所需的内容绘制在画布上
* 2.转化画布上的内容为 Data URI
* 3.将转化结果赋值给<a></a>元素标签的href属性

示例代码如下：

html

```
<p>转换前：</p>
<img id="image" src="./canvas-video/foo.png" width="50" height="50" alt="">

<p>画布：</p>
<canvas id="canvas" width="50" height="50"></canvas>

<p>转换后：</p>
<img id="new-image" src="" alt="">

<p>操作：</p>
<button id="transform">转换</button>
<a id="download" href="" download="ff">下载</a>
```

js

```
var download = document.getElementById('download')
var transform = document.getElementById('transform')
var image = document.getElementById('image')
var newImage = document.getElementById('new-image')

var canvas = document.getElementById('canvas')
var ctx = canvas.getContext('2d')

image.onload = function() {
	ctx.drawImage(this, 0, 0, 50, 50)
}

function canvasToImage(canvas) {
    var img = new Image()
	img.src = canvas.toDataURL('image/png')
	return img
}

transform.onclick = function() {
	console.log(canvasToImage(canvas))
	newImage.src  = download.href = canvasToImage(canvas).src
}
```

![实际效果图](https://github.com/lvzhenbang/article/blob/master/canvas/img/print.png)

上面的是特殊的图片资源，如果是一般的dom元素呢？很显然，我们需要考虑另外的方法，也即是在[canvas上绘制DOM对象](dom.md)。



在实际的开发中，我们往往不需要转换的步骤，而是直接下载。