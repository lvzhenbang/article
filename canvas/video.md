## canvas中引入video

canvas-video 的趋势是360，VR，全屏视频。

举例说明：

VR：[Mercedes-Benz](http://special.mercedes-benz.com.cn/thenewe-classlaunch/mobile/index.html)

全屏视频：[Haier – 书的 H5互动广告](http://cdn.im-ad.com/2016/HaierBook/?from=singlemessage&isappinstalled=0)

canvas-video也可以制作灰色视频，模糊背景，视频截图

### 在canvas中引入video的方式

第一种，drawImage()，代码如下：

html

```
<div>
	<video src="http://www.w3school.com.cn/i/movie.ogg" controls="controls">
		<p>你的浏览器不支持video标签</p>
	</video>
</div>
	
<canvas id="canvas" width="750"," height="600">
	<p>你的浏览器不支持canvas，请下载最新版本的浏览器</p>
</canvas>
```

js

```
var ctx = document.getElementById('canvas').getContext('2d')

var video = document.querySelector('video')

video.play()

setInterval(function() {
	ctx.drawImage(video, 100, 100, 100, 100)
}, 16)
```

![实际效果图](https://github.com/lvzhenbang/article/blob/master/canvas/img/video-1.png)

第二种，getImagedata()，putImageData()，制作灰色视频代码如下：

```
var ctx = document.getElementById('canvas').getContext('2d')
var ctxBack = document.createElement('canvas').getContext('2d')
var video = document.querySelector('video')
var cw, ch
video.addEventListener("play", function() {

    cw = video.clientWidth
    ch = video.clientHeight
    draw(video, ctx, ctxBack, cw, ch)
}, false)

function draw(video, ctx, ctxBack, cw, ch) {
    if(video.paused || video.ended) return false
    ctxBack.drawImage(video, 0, 0, cw, ch)
    var idata = ctxBack.getImageData(0, 0, cw, ch)
    var data = idata.data
    for(var i=0; i < data.length; i+=4) {
        var r = data[i + 0]
        var g = data[i + 1]
        var b = data[i + 2]

        var brightness = (3*r+4*g+b)>>>3

        data[i + 0] = brightness
        data[i + 1] = brightness
        data[i + 2] = brightness
    }

    idata.data = data

    ctx.putImageData(idata, 0, 0)

    setTimeout(function() {
        draw(video, ctx, ctxBack, cw, ch)
    }, 0)
}
```


![实际效果图](https://github.com/lvzhenbang/article/blob/master/canvas/img/video-2.png)

### 我们可以使用canvas做什么

1. 制作灰色视频

见上面的示例

2. 模糊背景

模糊背景的原理很简单，就是将canvas放置在video的底层。然后让视频居中，处理canvas的模糊。

这里的原理很简单就是使用drawImage()来缩放绘制的大小，然后控制样式让画布元素充满整个屏幕。

html

```
<canvas id="canvas"></canvas>

<video width="400" height="360" controls="controls">
	<source src="./movie.ogg" type="video/ogg"></source>
</video>
```

css

```
body {
    background-color: #000;
}

#canvas {
	width: 100%;
	height: 100%;
}

video {
	position: absolute;
	top: 50%;
	left: 50%;
	transform: translate(-50%, -50%)
}

```

js

```
document.addEventListener('DOMContentLoaded', function() {
	var canvas = document.getElementById('canvas')
	var video = document.getElementById('video')
	var ctx = canvas.getContext('2d')

	var cw = Math.floor(canvas.clientWidth / 100)
    var ch = Math.floor(canvas.clientHeight / 100)
    canvas.width = cw
    canvas.height = ch
	video.addEventListener('play', function() {
		draw(this, ctx, cw, ch)
	}, false)
}, false)

function draw(video, ctx, cw, ch) {
	if(video.paused || video.ended) return false

	ctx.drawImage(video, 0, 0, cw, ch)

	setTimeout(draw(video, ctx, cw, ch), 20)
}
```

[实际效果图](https://github.com/lvzhenbang/article/blob/master/canvas/img/blur.png)

3. 视频截图

视频截图想明白后，实现也比较容易。其实就是抓取某个时间点你所需的视频画面。

下面的例子我们抓取当视频播放后停止后端的视频画面。代码如下：

html

```
<div>
	<video id="video" width="400" height="300" controls>
		<source src="movie.ogg" type="video/ogg"></<source>
	</video>
</div>

<canvas id="canvas"></canvas>
```

js

```
var canvas = document.getElementById('canvas')          
var video = document.getElementById('video')
var ctx = canvas.getContext('2d')
var vCanvas = document.createElement('canvas')

var cw = video.width
var ch = video.height
canvas.width = vCanvas.width = cw
canvas.height = vCanvas.height = ch

document.addEventListener('DOMContentLoaded', function(){

	var cb = vCanvas.getContext('2d')

	video.addEventListener('play', function() {
		draw(video, cb, cw, ch)
	}, false)

	video.addEventListener('pause', function() {
		ctx.drawImage(vCanvas, 0, 0, cw, ch)
	}, false)
}, false)

function draw(v, cb, w, h) {
	if(v.paused || v.ended) return false
	
	cb.drawImage(v, 0, 0, w, h)

	setTimeout(draw, 0, v, cb, w, h)
}
```

![实际效果图](https://github.com/lvzhenbang/article/blob/master/canvas/img/grab.png)