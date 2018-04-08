## 在canvas上绘制DOM对象

在canvas上除了可以绘制图片、视频外，能不能绘制一般的dom对象吗？答案是肯定的，当然可以。
那么如何在canvas上绘制一般的dom对象能。

由于canvas支持将svg图像的绘制，所以可以将要绘制的HTML内容转化为svg图像，然后将转化后的svg会知道canvas中。

熟悉SVG的话，我们知道foreignObject元素可以访问外部的XML命名空间。

而要想将svg绘制在canvas上，我们需要获取svg对象的url，然后用drawImage()，将该对象绘制到画布上。

绘制svg对象我们使用createObjectURL(obj)，而在不需要的时候需要用revokeObjectURL()来手动释放它们，这样可以减少内存的占用，提升性能。如果不手动释放的话，它们也会在页面被关闭时被浏览器释放。

这里我们通过Blob对象来处理svg内容

实现代码：

```
var dom = document.getElementById('firefox')
var canvas = document.getElementById('canvas')
var ctx = canvas.getContext('2d')

function createForeignObject(width, height, x, y, node) {

	var xmlns = "http://www.w3.org/2000/svg"
	var svg = document.createElementNS(xmlns, 'svg')
	var foreignObject = document.createElementNS(xmlns, 'foreignObject')

	svg.setAttributeNS(null, 'width', width)
	svg.setAttributeNS(null, 'height', height)

	foreignObject.setAttributeNS(null, 'width', '100%')
	foreignObject.setAttributeNS(null, 'height', '100%')
	foreignObject.setAttributeNS(null, 'x', x)
	foreignObject.setAttributeNS(null, 'y', y)

	svg.appendChild(foreignObject)

	foreignObject.appendChild(node)

	return svg
}

function loadSvg(svg, width, height) {
	canvas.width = width
	canvas.height = height
	var img = new Image()

	var domUrl = window.URL || window.webkitURL || window

	var data = new Blob([new XMLSerializer().serializeToString(svg)], {type: 'image/svg+xml;charset=utf-8'})

	var url = domUrl.createObjectURL(data)

	img.src = url

	img.onload = function() {
		ctx.drawImage(this, 0, 0, width, height)
		domUrl.revokeObjectURL(url)
	}
}

window.onload = function() {
	var node = dom.innerHTML
	var cw = dom.clientWidth
	var ch = dom.clientHeight
	var svg = createForeignObject(cw, ch, 0, 0, dom)

	loadSvg(svg, cw, ch)
}
```

如果看过html2canvas源码的同学会发现它是通过data URI来处理svg内容，具体实现是通过encodeURIComponent(svg) 来编码svg内容。

代码修改后如下：

```
var dom = document.getElementById('firefox')
var canvas = document.getElementById('canvas')
var ctx = canvas.getContext('2d')

function createForeignObject(width, height, x, y, node) {

	var xmlns = "http://www.w3.org/2000/svg"
	var svg = document.createElementNS(xmlns, 'svg')
	var foreignObject = document.createElementNS(xmlns, 'foreignObject')

	svg.setAttributeNS(null, 'width', width)
	svg.setAttributeNS(null, 'height', height)

	foreignObject.setAttributeNS(null, 'width', '100%')
	foreignObject.setAttributeNS(null, 'height', '100%')
	foreignObject.setAttributeNS(null, 'x', x)
	foreignObject.setAttributeNS(null, 'y', y)

	foreignObject.appendChild(node)
	svg.appendChild(foreignObject)

	return svg
}

function loadSvg(svg, width, height) {
	canvas.width = width
	canvas.height = height
	var img = new Image()

	console.log(encodeURIComponent(new XMLSerializer().serializeToString(svg)))

	img.src = `data:image/svg+xml;charset=utf-8, ${encodeURIComponent(new XMLSerializer().serializeToString(svg))}`
	
	/*var domUrl = window.URL || window.webkitURL || window

	var data = new Blob([new XMLSerializer().serializeToString(svg)], {type: 'image/svg+xml;charset=utf-8'})

	var url = domUrl.createObjectURL(data)

	img.src = url*/

	img.onload = function() {
		ctx.drawImage(this, 0, 0, width, height)
		// domUrl.revokeObjectURL(url)
	}
}

window.onload = function() {
	var node = dom.innerHTML
	var cw = dom.clientWidth
	var ch = dom.clientHeight
	var svg = createForeignObject(cw, ch, 0, 0, dom)

	loadSvg(svg, cw, ch)
}
```

注意：由于svg必须是有效的XML，所以需要用XMLSerializer来解析HTML使它符合格式。
