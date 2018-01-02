## 掌握面试——弹出框的实现（一道题中包含布局/js设计模式）

这道面试题，当初我面试的时候被问过两次，因此比较深，此外，我记得还有设计模式的考察，所以，有深刻的体会。

### 面试题主要考察什么

面试不是个轻松的活，不管是对面试官还是面试者都一样。对于面试官来说，别的先不管，首先一点技术要过关，对候选人的基本要求就是基础扎实，有相关项目经验，有解决问题的能力，思路清晰，易于沟通。而对于面试者来说要技术扎实，知识面要广，要有技术闪光点，对于各种技术提起来都知道点，但经不起深层次的询问，这种‘一瓶子水不满，半瓶子水晃地’现象很不受面试官的喜欢。

虽然，我工作的时间不长，但被面试过，也面试过别人。所以，将这道题被问的情况和自己的体会理解分享给大家。

注：面试公司 去哪儿，360

### 用 html + css 实现一个弹出框

注：这是按照面试官的预想可能出现的情况设定的，不同的面试者临场发挥不一样，面试官可能问的问题也有所变化，但归根结底，这个系列的问题大致如下。

这里你在回答前需要问清楚实现有什么限制没，如果没有，你可以以任意的方式来实现；如果有，问清楚限制，一般限制有如下情况，一种定宽定高，另一种不定宽高（补丁宽高一般用js控制），但是使用css3亦可实现，如果你回答出一种，面试官往往会问有没有其他的实现方法，如果面试管这样问了，你如果，回答没有，会被扣分的。面试会进入下一个环节。

#### 弹出框垂直水平居中

html:

	<div class="box-default box-wh box-vc-mar">this is a pop-up box</div>

css:
	.box-default {
		position: fixed;
		top: 50%;
		left: 50%;
		z-index: 99;
		padding: 20px;
		background-color: white;
		border: 1px solid #ccc;
		border-radius: 8px;
	}

	.box-wh {
		width: 200px;
		height: 200px;
	}

	.box-vc-mar {
		margin-left: -100px;
		margin-top: -100px;
	}

[JSFiddle效果演示](https://jsfiddle.net/sanlv/jzry9g6q/)

#### 如果弹出窗的宽高不定

html:

	<div class="box-default">this is a pop-up box</div>

css:

	.box-default {
		position: fixed;
		top: 50%;
		left: 50%;
		z-index: 99;
		padding: 20px;
		background-color: white;
		border: 1px solid #ccc;
		border-radius: 8px;
	}

	.box-vc-mar {
		margin-left: -100px;
		margin-top: -100px;
	}

js:
	
	var $box = $('.box-default'),
	    bw = $box.width(),
	    bh = $box.height();
	$box.css({
	  marginLeft: - bw/2 + 'px',
	  marginTop: - bh/2 + 'px',
	});

[JSFiddle效果演示](https://jsfiddle.net/sanlv/rt4pa33q/)

#### 有没有其他方式实现不定宽高的弹出窗

答案：有

html:

	<div class="box-default box-vc-trf">this is a pop-up box that only html+css</div>

css:
	
	.box-default {
		position: fixed;
		top: 50%;
		left: 50%;
		z-index: 99;
		padding: 20px;
		background-color: white;
		border: 1px solid #ccc;
		border-radius: 8px;
	}

	.box-vc-trf {
		-webkit-transform: translate(-50%, -50%);
		-ms-transform: translate(-50%, -50%);
		-o-transform: translate(-50%, -50%);
		transform: translate(-50%, -50%);
	}

[JSFiddle效果演示](https://jsfiddle.net/sanlv/87j0pyy9/)

#### 4.如果给弹出窗加一个遮罩层该如何实现

html：

	<div class="box-default box-vc-trf">this is a pop-up box that only html+css</div>
	<div class="mask"></div>

css：

	.box-default {
	  position: fixed;
	  top: 50%;
	  left: 50%;
	  z-index: 99;
	  padding: 20px;
	  background-color: white;
	  border: 1px solid #ccc;
	  border-radius: 8px;
	}
	.box-vc-trf {
	  -webkit-transform: translate(-50%, -50%);
	  -ms-transform: translate(-50%, -50%);
	  -o-transform: translate(-50%, -50%);
	  transform: translate(-50%, -50%);
	}
	.mask {
	  position: fixed;
	  top: 0;
	  right: 0;
	  bottom: 0;
	  left: 0;
	  z-index: 98;
	  background-color: #000;
	  opacity: 0.75;
	  filter: alpha(opacity=75);
	}

[JSFiddle效果演示](https://jsfiddle.net/sanlv/prn479cb/)

下面的问题就问的比较深了，牵涉到了一些简单业务实现

### 根据是之前实现的 结果，如果在页面中有一个按钮，通过触发钮如何实现弹出窗以及实现关闭弹出窗

示例代码如下：

html:

	<div class="box-default box-vc-trf">this is a pop-up box that only html+css</div>
	<div class="mask"></div>

	<button id="btn">click it</button>

css:
	
	.box-default {
	  position: fixed;
	  top: 50%;
	  left: 50%;
	  z-index: 99;
	  display: none;
	  padding: 20px;
	  background-color: white;
	  border: 1px solid #ccc;
	  border-radius: 8px;
	}

	.box-in {
	  display: block;
	}

	.box-vc-trf {
	  -webkit-transform: translate(-50%, -50%);
	  -ms-transform: translate(-50%, -50%);
	  -o-transform: translate(-50%, -50%);
	  transform: translate(-50%, -50%);
	}

	.mask {
	  position: fixed;
	  top: 0;
	  right: 0;
	  bottom: 0;
	  left: 0;
	  z-index: 98;
	  display: none;
	  background-color: #000;
	  opacity: 0.75;
	  filter: alpha(opacity=75);
	}

	.mask-in {
	  display: block;
	}

js:

	var $box = $('.box-default'),
	    $mask = $('.mask'),
	    $btn = $('#btn');

	$btn.on('click', function(event) {
	  event.preventDefault();
	  $box.addClass('box-in');
	  $mask.addClass('mask-in');
	});

	$mask.on('click', function(event) {
	  event.preventDefault();
	  $(this).removeClass('mask-in');
	  $box.removeClass('box-in');
	});

[JSFiddle效果演示](https://jsfiddle.net/sanlv/mu3cg5bb/)

这个只是我的实现，每个人的实现都会有所不同。

此外，像这里如何关闭弹出窗的实现方式，并不一定要通过触发遮罩层来实现，我们常见的实现是在弹出窗中加一个关闭按钮。像在这里这种模棱两可的地方，往往存在陷阱，我建议在你给出答案前先问清楚这个关闭弹出窗要如何实现，并给出自己的方案，因为在实际的项目开发的过程有些细节问题产品经理并不会面面俱到，会有疏漏的地方，而作为一个前端你要及时的提出来，进行沟通确认，这里可能会考察你的观察，判断和沟通能力。但有些时候你问过后，面试官会对你笑笑，让你自由发挥，你这时就会明白，这是一处坑。但有时候不是这样，所以，面试时要主动与面试官沟通，避免因细节问题在面试过程中被降分。

### 如果在页面中有多个按钮，那么这个弹出窗要如何实现

同上，根据之前的建议在回答问题之前要问清楚问题中模棱两可的地方。

1.多个按钮是不是成百上千个，或者就是几个。

2.是否触发不同按钮弹出的窗口现实的内容不同。

同时也可以不问，你只要明白要考察的知识点即可，问只是让你更清楚的知晓面试官的考察点。

html:

	<div class="box-default box-vc-trf">this is a pop-up box that only html+css</div>
	<div class="mask"></div>

	<button class="btn">click</button>
	<button class="btn">click2</button>
	<button class="btn">click3</button>
	<button class="btn">click4</button>
	<button class="btn">click5</button>
	<button class="btn">click6</button>

css:
	
	css 同上个示例

js:

	$(function(){
		var $box = $('.box-default'),
			$mask = $('.mask');
	    
		$mask.on('click', function(event) {
		  event.preventDefault();
		  hideMask();
		});

		$('.btns').forEach(function(el, i) {
			el.on('click', function(event) {
			  event.preventDefault();
			  var content = getText($(this));
			  showMask(content);
			}
		});

		function getText(obj) {
			return obj.text()
		}

		function showMask(content) {
		  $box.addClass('box-in').html(content);
		  $mask.addClass('mask-in');
		}

		function hideMask(obj) {
		  $mask.removeClass('mask-in');
		  $box.removeClass('box-in');
		}
	});

这里主要考察了两点:

1.js基础是否扎实

* 表现和行为的分离
* 可维护性
* 可扩展性

2.是否使用了事件代理

代码修改如下：

	$(function(){
		...

		$(document.body).on('click', '.btn', function(event) {
		  event.preventDefault();
		  var content = getText($(this));
		  showMask(content);
		});

		...
	});

[JSFiddle效果演示](https://jsfiddle.net/sanlv/k8xwdpLd/)

有时候面试官会让你用原生js来实现事件代理，那么我们应该如何回答呢？

由于事件委托可以实现目标对象的隐藏，在开发这对于我们保护一些核心的对象非常有用，不过实话来说，在JavaScript中就是call，apply的使用，因为js中只有这两个方法提供了改变当前函数内部this作用域的功能。当然这只是对象的委托，而要实现对类的委托则要相对复杂些，对类的实现大家有兴趣的可以参考以下Prototype.js或Jquery中的相应实现。

事件代理就要改变侦听器的位置，或者说改变事件绑定的对象。得益于js的事件传播机制，实现起来非常容易。

下面这个例子大家都可能见过：

html:

	<ul id="nav">
		<li><a href="http://blog.csdn.net/weixin_41559723?ref=toolbar/">CSDN</a></li>
		<li><a href="https://segmentfault.com/u/birenyangguangcanlan">Segmentdefault</a></li>
		<li><a href="https://github.com/lvzhenbang">github</a></li>
		<li><a href="https://juejin.im/user/58b83c66128fe100642f5297/">稀土掘金</a></li>
		<li><a href="https://home.cnblogs.com/u/1309794/">博客园</a></li>
    </ul>

js:

	window.onload = function(){
	  var nav = document.getElementById("nav");
	  nav.onclick = function () {
	    var e = arguments[0] || window.event,
	        target = e.srcElement ? e.srcElement : e.target;
	    if (target.nodeName.toLowerCase() === 'a')
	    	alert(target.innerHTML);
	    return false;
	  }
	}

[JSFiddle效果演示](https://jsfiddle.net/sanlv/qdaofmu1/)

	var addEvent = (function () {
        if (document.addEventListener) {
          return function (el, type, fn) {
            el.addEventListener(type, fn, false);
          };
        } else if (document.attachEvent) {
          return function (el, type, fn) {
            el.attachEvent('on' + type, fn);
          }
        } else {
        	el['on' + type] = fn
        }
      })();

上面这个事件监听的兼容性代码片段，是面试官问我是否对jQuery源码有所了解，我当然说有，就让我说一下如何实现。

好了，关于事件代理的问题就不深入讨论了，我们接着了解关于弹出窗的其他问题。

### 遮罩层的共用问题

同一个页面我们可以考虑，直接在页面中加一个表示遮罩层的div，
但是对于不同的页面我们最好用js来控制。

问题：这里面试管会问，如何实现遮罩层的共享？

我们通常会写下面的一个代码片段：

	function createMask() {
		return $(document.body).append($('<div>').addClass('mask'));
	}

虽然，这样添加一个遮罩层，在隐藏的时候可以使用remove()移除它，但显然这样在页面中频繁的添加，删除dom元素不合理。

问题：有没有什么方法可以对它进行优化？

我们可能会考虑对它进行优化，先在全局创建一个这个遮罩层div，用一个变量来引用它。代码如下：

	var mask = $(document.body).append($('<div>').addClass('mask'));

这样页面就只会创建一个遮罩层div，但是可能出现，我们在使用的过程中不会用到这个div，这样我们就会造成资源的浪费，dom的节点就会平白多出一个无用的div。

这时我们可以借助一个变量来判断是否创建过div。代码如下：
	
	var mask;
	function createMask() {
		if (mask) {
			return mask;
		} else {
			mask = $('<div/>').addClass('mask').appendTo($(document.body));
			return mask;
		}
	}
	
细心的你是否发现这是一个单例？

当你在面试前对这个问题有研究，你说用一个单例模式来解决，面试官会让你先实现，然后让你说说单例模式的理解，最后询问相关的问题。如：什么是单例模式，单例模式有何优缺点，如何使用单例模式。

但是也有时候面试官会让你直接使用一个设计模式来对它进行优化？

在这里，面试官的询问方向跟面试官的知识面和掌控度有关。所以，面试大厂或者面试中/高级工程师的童鞋还是把自己的所学知识技能系统化比较好。


你发现上面那段代码实现有哪里不妥吗？对了，你在函数体内改变了函数外的变量mask的引用，在多人协作的项目中，你写的createMask是一个不安全的函数，此外我们开发中应尽量避免像mask这样的全局变量使用，用一个局部变量如何来解决这个问题呢，我想很多同学会想到闭包，修改后的代码如下：

	function createMask() {
		var mask;
		return function() {
			if (mask) {
				return mask;
			} else {
				mask = $('<div/>').addClass('mask').appendTo($(document.body));
				return mask;
			}
		}			
	}

好吧，到了这里你可能在心里想‘这下总算完了吧’，是的，我想说的是这道题你答成这个样子，对于中级工程师来说已经过关了，但是对于高级工程师来说，你还需要对这个代码进行优化，如果经常研究源码的童鞋会见到这样写的代码片段：

	function createMask() {
		var mask;
		return function() {
			return mask || mask = $('<div/>').addClass('mask').appendTo($(document.body));	
		}			
	}

对于那些开发框架，常用库或插件的大牛来说，同样的功能，同样的性能，不会多浪费一个字符。

如果是面试高级工程师，可能还会被问到，如何多个功能都要用到单例模式，该如何解决？

	function singleton(fn) {
		var res;
		return function() {
			return res || (res = fn.apply(this, arguments))
		}
	}

	var createMask = singleton(function () {
		return $('<div/>').addClass('mask').appendTo($(document.body));
	});

其实，这里又用到了另一种设计模式——桥接模式。

面试到这里一般就算完了。通过上面的代码我们发现，设计模式也不是什么洪水猛兽，只不过是设计模式的使用灵活多变，但要想真正完全掌握设计模式，不是看两篇文章就行的最重要的还是要多想，多实践。

[最终JSFiddle效果演示](https://jsfiddle.net/sanlv/wy89t32c/)
