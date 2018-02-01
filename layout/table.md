### 自定义表格

我们如何在html网页中是以一个类似excel表格的table，我们先来一张效果图：

![效果图](https://github.com/lvzhenbang/article/blob/master/layout/img/table.jpg)

这样的需求可以说比较常见，像这样的表格有两处我们需要注意，第一部分就是实现合并单元格，第二部分就是表头的左上角的文字分区域显示。


### 合并单元格

单元格合并分为行合并和列合并。

#### 列合并

![合并两列](https://github.com/lvzhenbang/article/blob/master/layout/img/table-col.jpg)

合并两列我们使用 `colspan` 属性，它的值代表所合并的列的个数。

```
<table border="1px solid #ccc">
	<tr>
    	<td colspan="2">12</td>
    	<td>12</td>
    	<td>12</td>	
	</tr>
	<tr>
    	<td>12</td>
    	<td>12</td>
    	<td>12</td>
    	<td>12</td>	
	</tr>
</table>
```

#### 行合并

![合并两行](https://github.com/lvzhenbang/article/blob/master/layout/img/table-row.jpg)

合并两行我们使用 `rowspan` 属性，它的值代表所合并的行的个数。

```
<table border="1px solid #ccc">
	<tr>
    	<td rowspan="2">12</td>
    	<td>12</td>
    	<td>12</td>	
	</tr>
	<tr>
    	<td>12</td>
    	<td>12</td>	
	</tr>
</table>
```

### 表头左上角的excel风格

主要有两部分，一部分是文字的分割线，另一部分是文字的布局。

#### 文字的布局

文字布局的实现方式比较固定，就是绝对定位。代码如下

html代码如下:

```
<th>
    <div class="content">
		<span class="content_left-bottom">序号</span>
		<span class="content_right-top">姓名</span>
	</div>
</th>
```

css样式如下：

```
th {
	position: relative;
}

.content>span {
    position: absolute;
    padding: 6px 8px;
}

.content_left-bottom {
    bottom: 0;
    left: 0;
}

.content_right-top {
    top: 0;
    right: 0;
}
```

这样就将文字放在了单元格的左下和右上角。

#### 分割线的实现

分割线的实现有多种方案。

不管使用那种方案，我们要定义这部分区域的素的宽和高。要实现这个对角分割线，我们需要知道 `th` 这个元素的宽高，我们可以设置它的内容的宽高，也即是类名为content元素的宽高。

```
.content{
	position: relative;
	width: 100px;
	height: 100px;
}
```

1.背景图片

在垒位content的元素中引入一张背景图片：

```
.content{
	width: 100px;
	height: 100px;
	background-image: url('../ling.png');
}
```

2.transform: rotate()

我们可以在 `<th></th>` 内再加入一个元素，也可以使用content类的伪元素（下面的几种方案我们统一使用伪元素）。

```
.content::after {
	position: absolute;
	top: 0;
	left: 0;
	width: xxx; // 它的宽我们需要计算
	height：1px;
	background-color: #000;
	transform-origin: 0 0;
	transform: rotate(45deg);
}
```

首先，计算角度。我们可以求出正弦或余弦值，然后使用反三角函数求出角度；

```
var tanx = 100/100;
var x = Math.atan(tanx)*180/Math.PI; // 45
```

接着，我们将计算为元素的宽

	Math.ceil(100/Math.sin(Math.PI/4)); // 142

伪元素的最终样式如下：

```
.content:after {
	position: absolute;
	top: 0;
	left: 0;
	display: block;
	width: 142px;
	height：1px;
	background-color: #000;
	transform-origin: 0 0; // 定义元素旋转的中心点
	transform: rotate(45deg);
}
```

最终代码如下：

```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>rotate</title>
	<style>
	.content{
		position:relative;
		width: 100px;
		height: 100px;
		border: 1px solid #ccc;
	}
	.content>span {
	    position: absolute;
	    padding: 6px 8px;
	}

	.content_left-bottom {
	    bottom: 0;
	    left: 0;
	}

	.content_right-top {
	    top: 0;
	    right: 0;
	}
	div.content:after {
		position: absolute;
		top: 0;
		left: 0;
		content: '';
		display: block;
		width: 142px;
		height: 1px;
		background-color: #000;
	    transform-origin: 0 0;
	    transform: rotate(45deg)
	}
	</style>
</head>
<body>
    <div class="content">
		<span class="content_left-bottom">序号</span>
		<span class="content_right-top">姓名</span>
	</div>
</body>
</html>
```

[效果预览](https://codepen.io/lvzhenbang/project/editor/ZepemN#table-transform.html)

3. 通过设置border的颜色来实现

修改伪元素的样式如下：

```
div.content:before {
	position: absolute;
	top: 0;
	left: 0;
	content: '';
	display: block;
	border-bottom: 100px solid #000;
	border-right: 100px solid transparent;
}
div.content:after {
	position: absolute;
	top: 1px;
	left: 0px;
	content: '';
	display: block;
	border-bottom: 99px solid #fff;
	border-right: 99px solid transparent;
}
```

设置border的样式生成两个三角形，然后让两个三角形错位形成分割线。这里我们要注意一个层级的问题，生成分割线的图形盖在了字体上挡住了字体，我们只需提高字体的层级即可。


[效果预览](https://codepen.io/lvzhenbang/project/editor/ZepemN#table-rect.html)

[仿excel效果table 完整demo](https://codepen.io/lvzhenbang/pen/QQjwMo)