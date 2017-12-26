## HTML5中的 `data-*` 如何处理数据详解

写过HTML5页面，或者做过H5开发的同学或多或少都接触过`data-*`这个自定义Attribute（对Attribute和property还分不太清的同学，可以看一看[傻傻的分也分不清楚的property和attribute](https://github.com/lvzhenbang/article/blob/master/js/porp-attr.md)）这篇文章。

在做微信公众号开发的时候你一定看到过下图的情况：

![微信](https://github.com/lvzhenbang/article/blob/master/img/js/weixin.png)

我之前做过一个微信图文编辑器，用到过不少这样的情况。

在一些电商网站的源码中也看到过如下图的情况：

![淘宝](https://github.com/lvzhenbang/article/blob/master/img/js/taobao.png)

在移动端的开发中，我们经常会使用这种情况，对媒体资源（图片，视频）的优化处理上

还有就是我们前端开发都接触过的bootstrap框架，在插件的引用上就是使用的 `data-*`，那么它的魅力何在呢？

### HTML5 `data-*` 由来

在HTML5出现之前我们处理一些被引入的数据是通过用的 `class Attribute` 或者元素自带的 attribute，或者开发人员自定义一些数据属性。

这样操作数据很不方便，如何在页面内处理数据更方便，成了开发人员苦恼的问题，在HTML5出现后你就不用在担心了。

[w3c data-attribute 规范](http://w3c.github.io/html/dom.html#custom-data-attribute)

### HTML5 `data-*` 如何工作

我们可以在一个列表相中存储一个用户的信息，如下

	<li data-id="111111" data-name="lvzhenbang" data-email="lvzhenbang@outlook.com" data-github="https://github.com/lvzhenbang">lvzhenbang</li>

这些数据对于页面的访问者来说用处不大，因为用户看不到，但是它对于web应用的开发很有用。这时你可以想象一下用一个删除按钮，这个按钮要删除的列表中的某个用户，该用户的数据信息 `data-id` 就在按钮中，你不需要其他的相关的东西，或者属性操作，就可以直接向后端发送请求。

	<button id="delete-btn" type="button" data-cmd="delete" data-id="111111">删除</button>

使用场景就是如上面描述的那样。

### 用 HTML5 `data-*` 能存储些什么

`data-*` 是个好东西，但它不是万能的，它本身还是 `[Attribute](https://github.com/lvzhenbang/article/blob/master/js/porp-attr.md)` ，而 `Attribute` 就是只能为字符串。如果你将一个对象存储在其中是不行的，但对对象进行序列化处理后，还是可以的。现在你只需要知道它只能存储字符串就行了。

#### 用JavaScript来reading/writing `data-*`

	// 获得用户信息的操作按钮
	var oBtn = document.getElementById('opt-btn');
	// 获得删除按钮
	var delBtn = document.getElementById('delte-btn');
	// 获得操作的信息
	var id = oBtn.getAttribute('data-id');
	// 改变删除按钮的数据信息
	delBtn.setAttribute('data-id', id);

是不是很简单，然后你就可以通过AJAX向后端请求，做你想要做的事情了。

#### 用jQuery来reading/writing `data-*`

在jQuery中有一个 `.attr()` 方法就可以解决读写data数据的功能了。

	// 获得用户信息的操作按钮
	var oBtn = $('#opt-btn');
	// 获得删除按钮
	var delBtn = $('#delte-btn');
	// 获得操作的信息
	var id = oBtn.attr('data-id');
	// 改变删除按钮的数据信息
	delBtn.attr('data-id', id);

熟悉jQuery的可能会想到不是还有一个 `.data()` 方法吗? 虽然，`.attr()` 和 `.data()` 在操作 `data-*` 上有一些重叠，但他们的完全是两回事。没有深入理解的同学，只要知道 `.attr()` 就好了。

#### HTML5 有原生的API `dataset` 来reading/writing `data-*`

HTML5 就是这样好用，但是存在一些兼容性问题，IE以下不支持。但是时代在进步，这些奇葩的浏览器最终会成为历史，所以还是有必要说两句。

	// 获得用户信息的操作按钮
	var oBtn = document.getElementById('opt-btn');
	// 获得删除按钮
	var delBtn = document.getElementById('delte-btn');
	// 获得操作的信息
	var id = oBtn.dataset.id;
	// 改变删除按钮的数据信息
	delBtn.setAttribute.id = id;

值得注意的是这里没有数据前缀和 `-`，类似于我们在JavaScript中操作css属性的方式，我们在使用dataset API时，你在HTML中用 `data-some-attribute-name`，但在JavaScript中你需要用 `dataset.someAttributeName` 这样的驼峰形式。

### 用 HTML5 `data-*` 能做些什么事情呢

这里有几个简单的例子，仅供参考学习。

#### 过滤

这其实是一个简化版的模糊查询，我曾经再一次面使用遇到过这样的问题，就是做一个简单的模糊查询，这我会在另一篇文章中单独讲。

假如你有一个列表的demo，你想要过滤他们的关键字。只要你将它们的关键字放入 `data attribute` 中，然后编写一个简短的脚本循环并显示/隐藏它们即可。
	
html
	
	<input type="text" id="filter">
	<ul class="person">
	    <li data-models="'王明', 25, '网络工程', '篮球'">王明</li>
	    <li data-models="'张华', 23, '软件工程', '足球'">张华</li>
	    <li data-models="'张华', 24, '计算机科学技术', '篮球'">王丽</li>
	    <li data-models="'王大牛', 25, '网络工程', '篮球'">王大牛</li>
	    <li data-models="'王小二', 25, '机电自动化', '篮球'">王小二</li>
	    <li data-models="'张丽', 24, '机电自动化', '篮球'">张丽</li>
	</ul>

js
	
	$('#filter').on('keyup', function() {
	    var keyword = $(this).val().toLowerCase();
	    $('.person > li').each( function() {
	        $(this).toggle( keyword.length < 1 || $(this).attr('data-models').indexOf(keyword) > -1 );
	    });
	});

[demo演示](https://jsfiddle.net/sanlv/w6c4xsf9/)

#### style 样式

毫无疑问你可以使用 `class` 来定义样式，但是你也可以用 `data-*` 来应用样式（不用管data数据的值）：

HTML
	
	<span class="label" data-warning>

CSS
	
	[data-warning] {
	    background: red;
	}

但是如果你想依据数据属性的值呢？你可以这样使用：

	[data-warning*=error] {
	    color: red;
	}

	[data-warning*=update] {
		color: green;
	}

	[data-warning*=modify] {
	    color: blue;
	}

上面的使用是不是很熟悉，相信研究过bootstrap的同学对此应该不会陌生。

可参考bootstrap中将[link转化为按钮](https://v3.bootcss.com/css/#buttons)

[demo演示](https://jsfiddle.net/sanlv/bvnwL202/)

#### 响应式开发

在做响应式开发的过程中我们不仅可以使用媒体查询，我们还可以用 `data-*`
	
	// html
	<div data-role="mobile">
		移动端内容
	</div>
	// css
	div[data-role="mobile"] {  
	  display:block;  
	}

#### configure js插件的配置项

BootStrap用自定义数据属性作为可选择的配置项来配置插件。一个 [popover](https://v3.bootcss.com/javascript/#popovers) 例子如下:

	<button type="button" class="btn btn-lg btn-danger" data-toggle="popover" title="Popover title" data-content="And here's some amazing content. It's very engaging. Right?">点我弹出/隐藏弹出框</button>

#### 和伪元素的结合实现tool Tip
	
	// html
	<span class="tooltip" data-tooltip="我是一个数据属性值">数据属性</span>
	
	// css
	.tooltip {
	  position: relative;
	  display: inline-block;
	  padding: 4px 8px;
	  color: white;
	  background-color: green;
	  border-radius: 2px;
	  cursor: pointer;
	}

	.tooltip::after {
	  position: absolute;
	  top: 110%;
	  left: 0;
	  content: attr(data-tooltip);
	  display: none;
	  width: 200%;
	  padding: 6px 12px;
	  color: white;
	  background-color: rgba(0, 0, 0, 0.75);
	  border-radius: 6px;
	}

	.tooltip:hover::after {
	  display: inline-block;
	}

[demo演示:](https://jsfiddle.net/sanlv/q1a7kt28/)

### 什么时候应该用

1.js动画计算中可能需要的元素初始宽高，透明度等样式

2.Flash加载Flash电影的的存储参数，也包括(img/video/audio)

3.在游戏开发中存储一些人物属性的数据

4.web统计标签数据的证明(也就是我们常用的给元素取一个为一名字，方便统计)

### 什么时候不应该用

1.不要用它来替换一个现有的属性或元素。如下：

	<span data-time="20:00">8pm<span>

我们应该像下面这样定义：

	<time datetime="20:00">8pm</time>

2.数据属性不应该用作meta data 和 micro format的替代品。

micro format 被设计给人类的，是被引入给我们的标记上下文的。例如：如果你有一张Vcard用来记录个人或组织的联系信息，那么你将会给这张Vcard一个类，让机器理解这是一个联系信息。代码如下：
	
	<div class="vcard">
	 	<span class="fn" >Aaron Lumsden</span>
	</div>

而不是像下面的代码：
	
	<div class="vcard">
	 	<span data-name="Aaron Lumsden " >Aaron Lumsden</span>
	</div>

想要了解更多的micro format您可以访问 [microformats.org](http://microformats.org/)

3.他仅限于网站或app的内部使用，而不能用在外部，外部的用一个XML/RSS


### 结束语

`data-*`在web上已经被广泛的应用。令人兴奋的是，他们在旧浏览器上依然运行良好，并遵循web的标准，这意味着你不用担心兼容性的问题。但是如果你试图用用 `data-*` 以便于样式获取data的话，那么只有支持css3选择器的浏览器支持这一功能。


想要了解更多你可以看看这些文章

[Working with HTML5 data attributes](https://www.abeautifulsite.net/working-with-html5-data-attributes)

[All You Need to Know About the HTML5 Data Attribute](https://webdesign.tutsplus.com/tutorials/all-you-need-to-know-about-the-html5-data-attribute--webdesign-9642)

[HTML5 Custom Data Attributes `data-*`](http://html5doctor.com/html5-custom-data-attributes/)
