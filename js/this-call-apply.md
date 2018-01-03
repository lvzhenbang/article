## this,call和apply(这三个东西，如何牢牢记住)

这三个东西虽然一直再用，也用的很顺手，知道它的用法，也知道它的区别，但是最近在攻克设计模式这个高地时总感觉缺点什么，没得办法，就只好重新学习一下。并总结了些许个人心得，分享给大家。

### this

跟别的语言不太一样，JavaScript的this总是指向一个对象，而具体指向那个对象又是基于函数的执行环境（有人理解为上下文）动态绑定的，不是函数被声明的环境，而是函数被引用的环境。

### this指向

这个在‘度娘’上一搜文章多的是，但是存在这样的问题，要么是总结的不全，要么是谈的是自己的理解，一个用途的总结就能写上几段不适用，没得办法只好翻墙Google一下，找到几篇不错的文章，文章链接请转到文章结尾参考资料部分。

个人感觉Google搜索出来的学习资料价值更高，国内的搜索引擎厂商估计都把精力用在如何赚用户的钱上了。

我们知道this指向对象，所以相对来说它的含义就比较丰富，它可以是全局对象，当前对象，或者任意对象，这完全取决于函数的调用方式。JavaScript中函数的调用有以下几种方式。

* 作为普通函数调用（全局对象）
* 作为对象的方法调用（当前对象）
* 作为构造函数调用（任意对象）
* Function.prototype.apply或Function.prototype.call的调用（任意对象）

#### 1.作为对象的方法调用

	window.name = 'globalName'; 
	var myObj = {
		name: 'localName',
		getName: function(){
			return this.name;
		}
	};	 
	myObj.getName(); // localName


#### 2.作为普通函数调用
	
	window.name = 'globalName'; 
	var getName = function(){
		return this.name;
	}; 
	 
	getName(); // globalName 

我们将对象的方法调用赋值给一个变量，这样将它转化为普通的方法，就会出现下面的结果

	window.name = 'globalName'; 
	var myObj = {
		name: 'localName',
		getName: function(){
			return this.name;
		}
	};	 
	var getName = myObj.getName;
	getName(); // globalName

相信大家都遇到过这样的情况，我们在操作dom节点时，一个局部的callback方法被当作不同函数使用时callback中的this指向window，而在实际的开发中，我们希望的是它能够指向window，我们往往这样做。

html

	<div id="div">this is a div</div>

js

	window.id = 'window'; 
	document.getElementById('div').onclick = function(){
		var that = this; // 保存 div 的引用
		// 被转换为不同函数的callback
		var callback = function() {
			alert ( that.id ); // 'div'
		}
		// 调用callback
		callback();
	}; 


#### 3.作为构造函数调用

众所周知，JavaScript时一门面向对象的语言，但与其他的面向对象编程语言不同，它并没有类（class）这个感念，取而代之的是基于原型（prototype）的继承方式。JavaScript的构造函数也很特别，如果没有new关键字，就和不同函数没有区别，在在开发的过程中构造函数的名字以大写字母开头，这里有些人注意到了，有些人没有注意到，或者注意到了也不知道为什么，虽然你写成小写也一样使用，但是这恰恰说明你的业余，在这里大写首字母就是为了协作开发，统一标准，同时也是再提醒调用者使用正确的方式调用，this才会绑定到新创建的对象上。

大部分的JavaScript函数都可以被当作构造器来使用。构造器的外表和普通函数一样，区别在于被调用的方式（new）。

我们应该注意构造函数经new调用后，如果不指定返回对象类型，那么默认返回this，如果指定了，就返回该指定的对象类型。返回指定类型对象的情况，我相信写过插件的童鞋都应该有或多或少的了解。

	var MyClass = function(){
		this.name = 'lisi';
		// 显式地返回一个对象
		return {
			name: 'lzb'
		}
	}; 
	var obj = new MyClass();
	obj.name; // lzb

注：显示的返回的数据类型需保证是对象，其他的类型没有用。

#### apply/call的调用

相信大家对JavaScript有了解的同学都知道‘JavaScript中一切皆对象’这句话。因此，函数也是对象，是对象就有方法，而apply和cal就是函数对象的方法，这两个方法很强大，它可以实现切换函数的执行环境（上下文），也即是可以改变this绑定的对象，这样的方法被广泛应用。

	function MyObj() {
		this.name = 'lisi';
		this.getName = function(){
			return this.name;
		}
	}; 
	var obj = new MyObj();
	var obj2 = {name: 'lzb'}
	// 将this指向的obj改为obj2
	obj.getName.apply(obj2); // lzb

#### 殊途同归

其实，作为开发人员的我不喜欢这样的总结，这不是原理的理解，而是把凡是所有涉及到this的使用进行了分类总结，看过[Understanding JavaScript Function Invocation and “this”](http://yehudakatz.com/2011/08/11/understanding-javascript-function-invocation-and-this/)文章后有点小收获，作者将apply/call作为函数调用的基本方式，其它的3种方式都是在这一基础上演变而来的，也就是我们常说的语法糖。

函数调用过程就是this的绑定过程，这4种情况都要完成这样一个绑定，不同的是作为一般函数调用时，this绑定的是全局对象；作为对象的方法调用时，this绑定到该方法所属的对象。

#### 找不到this

我们都见过这样的实现通过document.getElementById('div')获得元素对象，但是我们又嫌它使用起来太麻烦，所以，我们会将他进行封装用一个短函数来代替，如下所示：
	
	var getId = function(id) {
		return document.getElementById(id);
	};

	getId('div');

但是，有没有人这样用如下的方式实现过：
	
	var getId = document.getElementById;

	getId('div');

我想在没有接触第一种实现方法之前，有人肯定这样想过，这个方法肯定有人用过，不过之后还是使用了第一种，很明显这种方法实现是不可行，学过以上的知识我们知道document.getElementById 方法的内部实现要用到this，而在第二种中getId作为一个普通函数它的this指向window，所以getELementById中this指向window，但是我们知道getElementById方法是document对象的方法，方法要想正常使用应该指向document，而第二种方法指向了window，所以，第二种方法有问题，但是我们可以使用apply来修正,把document当作this传入getId，代码如下所示：

	document.getElementById = (function(fn) {
		return function () {
			return fn.apply(document. arguments)
		}
	})(document.getElementById);

	var getId = document.getElementById;

	getId('div');

这个可以参考dojo中的lang.hitch实现

	button.onclick = lang.hitch(myObject, myObject.handler);

### call 和 apply

在函数式编程风格的代码编写中，call和apply被广泛应用，在JavaScript的设计模式中这两个方法也是应用广泛。

#### call vs apply

apply接受两个参数，第一个参数指定了函数体内this对象的指向，第二个参数为一个带有下表的集合（它可以是数组，也可以是类数组），apply把集合中的元素作为参数传递给被调用的函数。

call接受的参数不固定，第一个参数与apply作用一样，剩余的参数传递给被调用的函数。

当一个函数被调用是，JavaScript的解释器并不会计较形参和实参在数量、类型、顺序上的差别，在js的参数内部是使用一个数组来标识的（apply的使用效率比call要高）。

在我们使用call/apply的时候如果第一个参数为null，函数体内的this指向全局对象，但是在严格模式下返回的还是null。

可以在控制台输入如下代码：

	var func = function(a) {
		console.log(this === window)
	}

	func.apply(null) // true

	// 严格模式
	var func = function(a) {
		"use strict";
		console.log(this === null)
	}

	func.apply(null) // true

### call/apply 的用途

通过上面的例子我们已经知道了他们的第一个用途就是改变函数内部的this的指向

我们在做html的交互工作时，所写的js都要求功能和实现的分离。

	var oDiv = document.getElementById('div');
	oDiv.onClick = function() {
		func.apply(this)
	};

	function alertId() {
		alert(this.id)
	}

### Function.prototype.bind()

这个方法自大多数高版本浏览器中都实现了，它的作用就是用来指定函数内部的this指向，虽然存在兼容性，但是在我们知道了call/apply 的作用之后，我们可以自己写一个，代码如下：

	Function.prototype.bind = function (context) {
		var that = this; // 保存原函数的this
		return function() {
			return that.apply(context, arguments) // 执行函数前会将context传入函数体内当作this
		}
	};
	
	var obj = {
		name: 'lzb'
	};

	var getName = function() {
		console.log(this.name)
	}.bind(obj);

	getName(); // lzb

在这里我么们还可以向传递其他参数，代码修改如下：

	Function.prototype.bind = function() {
		var that = this;
		var context = [].shift.apply(arguments);
		var args = [].slice.apply(arguments);

		return function() {
			return that.apply( context, [].concat.apply( [].slice.apply( arguments ), args) )
		}
	}

	var obj = {
		name: 'lzb'
	};

	var setName = function(name) {
		this.name = name;
	}.bind(obj, 'xxx'); // 设置setName 的默认参数

	var getName = function() {
		console.log(this.name)
	}.bind(obj);

	setName(); // 默认为 xxx
	// setName('snalv');
	getName(); // sanlv

这里我们可以用bind实现更多更复杂的操作，这里就不一一介绍了。

### 参考资料

[Understanding the “this” keyword in JavaScript](https://toddmotto.com/understanding-the-this-keyword-in-javascript/)

[深入浅出 JavaScript 中的 this](https://www.ibm.com/developerworks/cn/web/1207_wangqf_jsthis/index.html)

[Understanding JavaScript Function Invocation and "this"](http://yehudakatz.com/2011/08/11/understanding-javascript-function-invocation-and-this/)
