## 在canvas中引入图片

canvas API提供了drawImage(image, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight)

* 参数image为canvas图像资源（image，video，canvas等）
* 参数sx为原图像上的x轴位置
* 参数sy为原图像上的y轴位置
* 参数sWidth为原图像的截取宽度
* 参数sHeight为原图像的截取高度
* 参数dx为canvas上的x轴位置
* 参数dy为canvas上的y轴位置
* 参数dWidth为canvas上绘制图像的宽度
* 参数dHeight为canvas上绘制图像的高度

### 绘制图像

我们常见的两种方式：

一种是引入页面已经存在的图像。代码如下：

html

```
<img src="https://avatars3.githubusercontent.com/u/19884132?s=400&u=726a55af181fa2f01d54cdf0be894e68e546c568&v=4" width="100" height="100" alt="">
	<canvas id="canvas" width="750"," height="600">
		<p>你的浏览器不支持canvas，请下载最新版本的浏览器</p>
	</canvas>
```

js

```
var ctx = document.getElementById('canvas').getContext('2d')

window.onload = function() {
	var img = document.querySelector('img')

	ctx.drawImage(img, 100, 100, 100, 100)
}
```

注：需要页面中的图像元素加载完毕，然后再在画布上绘制。也可等待要要绘制的对象加载完毕，然后在画布上绘制，而不必等待所有的元素加载完毕。

另一种是引入页面不存在的图像。代码如下：

```
var ctx = document.getElementById('canvas').getContext('2d')

var img = new Image()
img.src = "https://avatars3.githubusercontent.com/u/19884132?s=400&u=726a55af181fa2f01d54cdf0be894e68e546c568&v=4"

img.onload = function() {
	ctx.drawImage(img, img.width, img.height)
}
```

