## canvas 中的图形变换

熟悉PS的都知道PS中有一个图形变换工具快捷键为`ctrl + T`，那么在canvas中有没有提供API使所绘制的图形发生变换呢，答案是有，只不过是没有ps功能强大。如果了解CSS3动画的话，对了解canvas的使用将变得容易。

缩放: scale(x, y), transform(x, 0, 0, y, 0, 0)
移动：translate(x, y), transform(1, 0, 0, 1, x, y)
倾斜：transform(0, x, y, 0, 0, 0)
旋转：rotate(angle)

### scale(x, y) 

参数x,y分别代表水平和垂直方向，可取任意数值

```
var img = new Image()
img.src = "https://avatars3.githubusercontent.com/u/19884132?s=400&u=726a55af181fa2f01d54cdf0be894e68e546c568&v=4"

img.onload = function() {
	ctx.drawImage(img, 0, 0, img.width, img.height)
}
// 将图像放大两倍
ctx.scale(2, 2)
```

### rotate(angle)

参数angle分别代表旋转的弧度，可取正值和负值。正值代表顺时针方向，负值代表逆时针方向。

```
var img = new Image()
img.src = "https://avatars3.githubusercontent.com/u/19884132?s=400&u=726a55af181fa2f01d54cdf0be894e68e546c568&v=4"

img.onload = function() {
	ctx.drawImage(img, 100, 100, 100, 100, 130, 30, img.width, img.height)
}
// 顺时针旋转30度
ctx.rotate(30*Math.PI/180)
```

### translate(x, y)

参数x,y分别代表水平和垂直方向，可取任意数值

```
var img = new Image()
img.src = "https://avatars3.githubusercontent.com/u/19884132?s=400&u=726a55af181fa2f01d54cdf0be894e68e546c568&v=4"

img.onload = function() {
	ctx.drawImage(img, 0, 0, img.width, img.height)
}
// 将图像水平移动x
ctx.translate(200, 0)
```

### transform(a, b, c, d, e, f)

参数a,d代表缩放；参数b,c代表倾斜；参数代e,f表位移。

```
var img = new Image()
img.src = "https://avatars3.githubusercontent.com/u/19884132?s=400&u=726a55af181fa2f01d54cdf0be894e68e546c568&v=4"

img.onload = function() {
	ctx.drawImage(img, 0, 0, img.width, img.height)
}
// 将图像垂直倾斜
ctx.transform(1, 0, 0.5, 1, 0, 0)
```