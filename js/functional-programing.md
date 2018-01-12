### javascript的函数式语言特性

我们知道JavaScript使一门面向对象的编程语言，但这门语言同时拥有很多函数式语言的特性。

JavaScript的设计者在设计最初就参考了LISP方言之一的Scheme，引入了Lambda表达式、闭包、高阶函数等内容，正是因为这些特性让JavaScript灵活多变。

### Lambda(匿名函数)表达式

lambda在JavaScript中通常被引用做匿名函数使用，被用作一个值传递给其他函数，或者把一个行为当作值来传递。

在ES6之前，我们使用这样的函数表达式，我们可以将一个匿名函数指定给一个变量。

	var add = function(a, b) { return a + b }

而在ES6中，我们使用箭头函数，它的语法更灵活，它有一些新的特性和陷阱。
	
	// 我们可以写成下面的形式
	var add = (a, b) => a + b;
	// 或者
	var add = (a, b) => { return a + b };

箭头函数的优势就是它没有自己的this，我们往往会遇到匿名函数的作用域特殊处理的情况，如果使用箭头函数就可以避免这样的情况。
	
	var id = 'global';
	var obj = {};

	obj.id = 'inner';
	obj.delayWork = function() {
		setTimeout(function() {
			console.log(this.id);
		})
	}
	obj.delayWork(); // global

我们的本意是想让对象调用方法输出它的属性id，结果输出的确是全局属性window的id，如下面使用箭头函数即可输出正确的结果；

	var id = 'global';
	var obj = {};

	obj.id = 'inner';
	obj.delayWork = function() {
		setTimeout(() => {
			console.log(this.id);
		})
	}
	obj.delayWork(); // inner

在这里是箭头函数的优势，但是在没有出现箭头函数前我们用的方法是：

	var id = 'global';
	var obj = {};

	obj.id = 'inner';
	obj.delayWork = function() {
		var that = this;
		setTimeout(function () {
			console.log(that.id);
		})
	}
	obj.delayWork(); // inner

这种方法有些人称为that方法，但是我们看英文的话都是用 `jumping this` , 很明显这里的意思就是跳出this的对象指代，我们可以在我们能确定this指代的对象的地方用that保存this，后面用到this都用that来取代。

箭头函数的短板：

* 在函数内不能使用call,apply来改变函数的内this
* 函数没有arguments

关于this,apply/call理解不太深的可以参考这表篇文章[this,call和apply(这三个东西，如何牢牢记住)](https://github.com/lvzhenbang/article/blob/master/js/this-call-apply.md)

两种形式的lambda的使用各有优略势，上面的示例就是两种lambda的搭配使用。

### 闭包

闭包了解了不一定就懂，懂了也并不一定会很好的使用，对于JavaScript程序员来说，它就是一座大山，前端开发人员需要迈过去的大山。

理解闭包，需要理解闭包的形成与变量的作用域以及变量的生存周期。

#### 变量的作用域

变量的作用域就是指变量的有效范围。

在函数中生命的变量的时候，变量前带关键字var，这个变量就会成为局部变量，只有在该函数内部才能访问这个变量；如果没有var关键字，就是全局变量，我们要注意这样的定义变量会造成命名冲突。

补充一点，函数可以用来创建函数作用域，有人不认为应该把函数当做作用域理解，认为函数就是一个代码块。我更倾向于后者，这里只不过是给大家补充点小知识。在函数里我们可以使用函数外的变量，但是在函数外却不能使用函数内的变量。对JavaScript的原型有深刻理解的同学都会明白，它会顺着原型链（有人会称之为作用域链）逐层向外搜索，一直到全局对象位置，所以是不能通过原型链向内搜索的。

#### 变量的生存周期

对于全局变量来说，它的生存周期是永久的，除非手动的销毁这个全局变量。

而对于局部变量来说，当函数调用结束的时候就会被销毁。

我们知道在开发的过程中我们不想定义更多的全局变量污染全局环境，我们又想使变量拥有永久的生存周期，同时我们又要变量的私有化。在这样矛盾的开发需求下，JavaScript闭包应运而生。

 	var cAlert = function() {
	 	var a = 1;
	 	return function(){
	 		a++;
	 		alert(a)
	 	}
	 }
	var f = cAlert();
	
	f();

这是一个常见的闭包例子，但实现上面的效果我们也可以这样做：

	var myNameSpace = {}; // 许可的全局命名空间

	myNameSpace.a = 1;

	myNameSpace.alert = function() {
		this.a++;
		alert(this.a)
	};
	
	myNameSpace.alert();

对于 `a` 我们可以有两种方法让它像全局变量一样拥有永久的生命周期，一种是使用闭包，一种是使用对象的属性，因为它们分别在全局的f，myNameSpace中被引用，所以他们的生命周期得以延长，因为众所周知，全局的生命周期是永久的；它们都是在全局变量下被定义，因此，保持了私有性；避免了全局污染。

虽然第二种方法也可以实现这种好处，但是你依然离不开闭包，闭包是从可以进行局部处理，而第二种方法它是从全局入手的。如：我们操作一个有序列表，单击每一项弹出他们的索引。代码如下：
	
	<ul>
		<li>0</li>
		<li>1</li>
		<li>2</li>
	</ul

	var oLi = document.querSelectorAll( 'li' ); 

    for ( var i = 0, len = oLi.length; i < len; i++ ){
    	(function(i){
			oLi[i].onclick = function(){
	    		alert (i);
	    	}
    	})(i)
    };

关于闭包的其它知识点你可以看[老生常谈之闭包](https://github.com/lvzhenbang/article/blob/master/js/closure.md) 这篇文章，这篇文章JavaScript语言的函数特性。

### 高阶函数

我记得一次面试时就被问到高阶函数，题目的大致意思什么是高阶函数，高阶函数有什么特性，谈谈你对高阶函数的理解。说实话，当时我根本就不知到什么是高阶函数，只知道JavaScript函数的特殊用法，就说了在闭包中函数可以当做返回值来用，函数可以当作另一个函数的参数被引用等。虽然知道这些，但是不知道为什么这样用，知道是因为接触过闭包，用过一些JavaScript的对象方法如：`Array.prototype.reduce(callback[, initial])` 等这样的JavaScript内置方法，但‘知其然，不知其所以然’。然后就是谈自己的感受，因为只会使用，所以说不出个所以然来。

高阶函数不是JavaScript的所特有的，其他编程语言也有。JavaScript中高阶函数和其他语言一样要满足如下两个条件：

* 函数可以作为参数被传递
* 函数可以作为返回值被输出

这两点的使用在JavaScript中很常见，有的同学可能就不以为然，不就是这吗，我在平常开发中经常见到，但是问题往往不敢发散，特别是面试的时候，知道归知道，用过归用过，但能不能说出个一二三来，就是另一回事，在面试的时候，面试官往往不是问你概念的，你不知道有时也没事儿，但是你要理解如何用，以及用它能干些什么。

下面就分别介绍一下它们的应用场景。

#### 函数被作为参数传递

把函数作为参数传递，是因为在开发中我们有很多易变的业务逻辑，如果对于这部分易变的业务逻辑我们可以把它当作参数处理，这样就大大的方便了我们的开发。就如同我们在日常开发中遵循的将业务中变化的部分和不变的部分分开一样（业务分离）。

##### 回调函数

向页面body内添加一个div，然后设置div元素隐藏。

```
function appendDiv(){
	var oDiv = document.createElement('div');
	oDiv.className = 'myDiv';
	oDiv.style.display = 'none';
	document.body.appendChild(oDiv);
}

appendDiv();
```

在日常的开发中我们经常见到有人这样实现，虽然达到了目的，但是为了实现代码片段的可复用性，我们应尽量避免硬编码的情况出现。

有时在面试中往往会遇到面试官让我们写一些看起来很简单的实现，就像上面的情况，这种答案虽然正确，但不是面试官所想要的答案，面试官会用其他的问题来验证你是否是他需要的开发人员。

为了达到代码的可复用和可维护，我们可以这样实现；

```
function appendDiv(callback){
	var oDiv = document.createElement('div');
	oDiv.className = 'myDiv';
	if(callback && typeof callback === 'function') {
		callback.call(null, oDiv);
	}
	document.body.appendChild(oDiv);
}

appendDiv(function(node) {
	node.style.display = 'none'
});
```

上面的代码是不是很熟悉，相信这样的代码对于经常学习研究源码的你来说并不陌生。

##### JavaScript中的内置方法的参数

就像我们经常使用的数组内置函数 `Array.prototype.filter()` ，`Array.prototype.reduce()` ，`Array.prototype.map()` 等一样把变化的的部分封装在回调函数中一样，在开发中我们也要经常使用这样的形式，告别硬编码。

```
var arr = [1, 2, 3, 4, 5];

var newArray = arr => arr.filter((item) => item%2 === 0);
newArray(arr); // [2, 4]
```

函数作为回调函数使用场景还有很多，比如，在开发中使用的Ajax请求等。

#### 函数作为返回值输出

函数作为返回值在我们的开发中也比较常见，例如我们说熟悉的闭包，这里就不介绍闭包了，我们用一个对象数组排序的例子来说明一下：

```
var personList = [
	{name: '许家印', worth: '2813.5', company: '恒大集团'},
	{name: '马云', worth: '2555.3', company: '阿里巴巴'},
	{name: '王健林', worth: '1668.2', company: '大连万达集团'},
	{name: '马化腾', worth: '2581.8', company: '腾讯'},
	{name: '李彦宏', worth: '1132', company: '百度'}
];
// 排序规则
function compareSort(item, order) {
	// 排序规则的具体实现
	return function(a, b) {
		if(order && oder === 'asc') {
			return a[item] - b[item]
		} else {
			return b[item] - a[item]
		}
	}
}
// 用compareSort的参数来实现自定义排序
personList.sort(compareSort('worth', 'desc'));

/*
[{name: "许家印", worth: "2813.5", company: "恒大集团"},
{name: "马化腾", worth: "2581.8", company: "腾讯"},
{name: "马云", worth: "2555.3", company: "阿里巴巴"},
{name: "王健林", worth: "1668.2", company: "大连万达集团"},
{name: "李彦宏", worth: "1132", company: "百度"}]
 */
```

我们在开发中经常会遇到这样的数据排序——自定义排序。

### 高阶函数AOP (面向切面编程)

面向切面编程这种思想在开发中比较常见，主要就是将一些与核心业务无关的功能抽离出来，比如异常处理，日志统计等。在开发中根据需要我们再通过 `动态织入` 的方式将这些分离出来的功能模块掺入业务逻辑块中。

这样做不仅可以保持业务逻辑模块的纯净和高内聚，还可以方便我们复用分离的模块。

我发现以前学习jQuery的源码只是学习了源码的功能实现。通过对比学习，我慢慢开始尝试理解jQuery的业务实现和架构组成。

在JavaScript这个基于 `prototype` 的动态语言实现面向切面编程很简单。

```
Function.prototype.success = function(fn) {
	var that = this;
	return function() {
		var ret = that.apply(this, arguments)
		fn.apply(this, arguments);
		return ret;
	}
};

Function.prototype.fail = function(fn) {
	var that = this;
	return function() {
		var ret = that.apply(this, arguments)
		fn.apply(this, arguments);
		return ret;
	}
};

function ajax () {
	console.log('get it.')
}

var get = ajax.success(function() {
	console.log('success');
}).fail(function() {
	console.log('fail');
});

get();
```

这里模拟了一个jQuery的Ajax的形式实现，有一个核心模块 `ajax` ，我们我将 `success`，`fail` 模块分离出来，这样就可以实现模块的 `动态织入` 。

### 高阶函数的应用

#### 函数节流

在开发中有些函数不是用户直接触发控制的，在这样的情况下就可能出现函数被频繁调用的情况，这样往往会引起性能问题。

函数频繁调用的场景归纳：

* window.onresize事件

当调整浏览器窗口大小时，这个事件会被频繁出发，而在这个时间函数中的dom操纵也会很频繁，这样就会造成浏览器卡顿现象。

* mousemove事件

当被绑定该事件的dom对象被拖动时，该事件会被频繁触发。

所以，函数节流的原理就是在不影响使用效果的情况下降低函数的触发频率。

```
var throttle = function ( fn, interval ) { 
    var __self = fn,  // 保存需要被延迟执行的函数引用
    	timer,        // 定时器
    firstTime = true; // 是否是第一次调用 
 
    return function () {
    	var args = arguments,
    		__me = this;
    	// 如果是第一次调用，不需延迟执行
        if ( firstTime ) {
	        __self.apply(__me, args);
	        return firstTime = false;
	    } 
 		// 如果定时器还在，说明前一次延迟执行还没有完成
        if ( timer ) {
        	return false;
        } 
 		// 延迟一段时间执行
        timer = setTimeout(function () {
        	clearTimeout(timer);
        	timer = null;
        	__self.apply(__me, args); 
        }, interval || 500 ); 
    }; 
}; 
 
window.onresize = throttle(function(){
	console.log(1);
}, 500 ); 
```


#### 分时函数

在开发中有时我们也会遇到，是用户主动触发的操作，倒是浏览器的卡顿或假死。例如，我用户批量操作向页面添加dom元素时，为了防止出现浏览器卡顿或假死的情况，我们可以每隔几秒向页面添加固定数量的元素节点。

```
// 创建一个数组，用来存储添加到dom的数据
var dataList = [];
// 模拟生成500个数据
for (var i = 1; i <= 500; i++) {
	dataList.push(i);
}
// 渲染数据
var renderData = timeShareRender(dataList, function(data) {
	var oDiv = document.createElement('div');
	oDiv.innerHTML = data;
	document.body.appendChild(oDiv);
}, 6);
// 分时间段将数据渲染到页面
function timeShareRender(data, fn, num) {
	var cur, timer;
	var renderData = function() {
		for(var i = 0; i < Math.min(count, data.length); i++) {
			cur = data.shift();
			fn(cur)
		}
	};

	return function() {
		timer = setInterval(function(){
			if(data.length === 0) {
				return clearInterval(timer)
			}
			renderData()
		}, 200);
	}
}
// 将数据渲染到页面
renderData();
```

[demo演示](https://jsfiddle.net/sanlv/npsnnfdj/)

#### 惰性加载函数

在web开发中，因为浏览器的差异性，我们经常会用到嗅探。那就列举一个我们比较常见的使用惰性加载函数的事件绑定函数 `removeEvent` 的实现：

一般我们都这样写：

```
var removeEvent = function(elem, type, handle) {
	if(elem.removeEventListener) {
		return elem.removeEventLisener(type, handle, false)
	}
	if(elem.detachEvent) {
		return elem.detachEvent( 'on' + type, handle )
	}
}
```

但是我们却发现jQuery 中是这样实现的

```
removeEvent = document.removeEventListener ?
	function( elem, type, handle ) {
		if ( elem.removeEventListener ) {
			elem.removeEventListener( type, handle, false );
		}
	} :
	function( elem, type, handle ) {
		if ( elem.detachEvent ) {
			elem.detachEvent( "on" + type, handle );
		}
	}
```

jQuery的写法避免了每次使用 `removeEvent` 都要进行的多余的条件判断，只要进行第一次的嗅探判断，第二次就可以直接使用该事件，但是前一种则是需要每次都进行嗅探判断，所以第二种的写法在开销上要比第一种低的多。
