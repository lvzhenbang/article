## 在背景中添加图案

在canvas中提供了一个API可以让我们在背景中添加使用图案。

方法为：

CanvasRenderingContext2D.createPattern(image, repatition)

第一个参数image为可作为重复图像源canvasImageSource对象：

```
HTMLImageElement(<img>)
HTMLVideoElement(<video>)
HTMLCanvasElement(<canvas)
CanvasRenderingContext2D,
ImageBitmap
ImageData
Blob

```

第二个参数 repetition

```
"repeat"：两个方向
"repeat-x"：水平方向
"repeat-y"：垂直方向
"no-repeat"：不重复
```

### 实例代码

```
var ctx = document.getElementById('canvas').getContext('2d');
var img = new Image();
img.src = 'https://mdn.mozillademos.org/files/222/Canvas_createpattern.png';
img.onload = function() {
  var pattern = ctx.createPattern(img, 'repeat');
  ctx.fillStyle = pattern;
  ctx.fillRect(0,0,400,400);
}
```