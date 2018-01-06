### javascript的函数式语言特性

我们知道JavaScript使一门面向对象的编程语言，但这门语言同时拥有很多函数式语言的特性。

JavaScript的设计者在设计最初就参考了LISP方言之一的Scheme，引入了Lambda表达式、闭包、高阶函数等特性，正是因为这些特性让JavaScript灵活多变。

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

这是一个常见的闭包例子，但是实现上面的效果，我们也可以这样做：

	var myNameSpace = {}; // 许可的全局命名空间

	myNameSpace.a = 1;

	myNameSpace.alert = function() {
		this.a++;
		alert(this.a)
	};
	
	myNameSpace.alert();

对于a我们可以有两种方法让它像全局变量一样拥有永久的生命周期，一种是使用闭包，一种是使用对象的属性，因为它们分别在全局的f，myNameSpace中被引用，所以他们的生命周期得以延长，因为众所周知，全局的生命周期是永久的；它们都是在全局变量下被定义，因此，保持了私有性；避免了全局污染。

虽然第二种方法也可以实现这种好处，但是你依然离不开闭包，闭包是从可以进行局部处理，而第二种方法它是从全局入手的。如：我们操作一个有序列表，单击每一项弹出他们的索引，代码如下：
	
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