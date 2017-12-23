## 老生常谈之闭包

闭包是什么？这是一个在面试的过程中出现的概率为60%以上的问题，也是我们张口就来的问题。但是我们往往发现，在面试的过程中我们的回答并不那么让面试官满意，我们虽然能张口说出一些但是却不能系统的对这个问题进行回答。面试官希望加入自己团队的是一个基础扎实，条理清晰，举一反三，吃苦耐劳的人，如果我们在面试中的回答只是‘东一榔头，西一棒槌’，结果我想已经显而易见了。

接下来将从三个方面来说闭包：

1.闭包是什么？

2.为什么要用闭包？

3.如何使用闭包？

### 闭包是什么

> 定义

闭包就是外层函数的内部函数（不过它有如下特性）。

> 特性

1.它有自己的局部作用域（local scope）;

2.它可以访问外部函数的作用域（outer scope），参数（parameters）,而不是参数对象;

3.它也可以访问全局的（global scope）

4.参数和变量不会被垃圾回收机制回收（不当的使用闭包可能造成内存泄漏的原因）

### 闭包与作用域

> javascript的作用域

javascript 是一个函数级的作用域（function-level scope）,而不是一个像其他语言一样的块级作用域（block-level-scope）；此外javascript是一个异步的事件驱动语言（javascript是单线程的语言），想要了解更多的异步事件驱动我们可以看一下[node.js的](http://nodejs.cn/)

> 闭包工作原理

1.闭包存储外部函数变量的引用，因此总是可以访问外部变量的更新值

2.在它的外部函数被执行并返回值后，闭包仍然可以执行(常驻内存)

### 为什么要用闭包

例如：在for循环中我们访问一个变化的变量时存在问题

而我们根据闭包的特性和工作原理可以很好的解决这个问题

> 闭包的好处

1.保存状态(使一个变量长期驻扎在内存中)

2.避免全局变量的污染

3.允许私有成员的存在
	
### 如何使用闭包

通常在函数返回时会失去对变量的访问权限，如下面的示例：
	
	function unClosure() {
	    var innerVar = "I'm inner-variable";
	    return innerVar;
	}
	unClosure(); // returns "I'm inner-variable"

上面的示例代码没有多少的实际意义，我们做一下修改，如下：
	
	function unClosure(outerVar) {
	    return outerVar;
	}
	unClosure();

	unClosure('apple'); // return "apple";
	unClosure('banana'); // return "banana";
	unClosure('tomato'); // return "tomato";

但是我们发现，在unClosure运行之后，我们将没有办法去获得outerVar。当然，我们可以再一次运行unClosure一次又一次，但是每一次调用之后将会创造一个新的outerVar，这样很难保证每一个outerVar是最新的，因为当前的调用没有存储任何引用。所以，我们可以将代码修改如下：
	
	function aClosure() {
	    var longLivedVariable = "I'm here for a long time";
	    var innerFunction = function inner() {
	        return longLivedVariable;
	    }
	    return innerFunction;
	}
	var closure = aClosure(); // 返回一个innerFunction的引用
	closure(); // returns "I'm here for a long time"

我们发现aClosure没有返回longLivedVariable，而是返回innerFunction的引用。这也即是说有一个引用一直挂载在innerFunction上，由于innerFunction上有一个longLivedVariable的引用，那么变量将一直存在。为了说明以上的推论，我们将代码修改如下：

	function aClosure(longLivedVariable) {
	    var innerFunction = function inner() {
	        return longLivedVariable;
	    }
	    return innerFunction;
	}
	var closure = aClosure('apple'); // 返回一个innerFunction的引用
	closure(); // returns "apple"
	closure(); // returns "apple"
	closure(); // returns "apple"
	var closure2 = aClosure('banana'); // 返回一个innerFunction的引用
	closure2(); // returns "banana"
	closure2(); // returns "banana"
	closure2(); // returns "banana"

当我们钓友closure()，我们一直调用的是innerFunction()，或者说是aClosure()的返回值，而对于innerFunction来说返回的是longLivedVariable.

上面的例子是保存状态的简单使用，下面我们来一个保存状态和允许拥有私有成员的例子,
这个常见的就是对象。示例代码如下：

	function person(name) {
	    return {
	        getName: function() {
	            return name;
	        }
	    }
	}

	var person = person('lisi');
	person.name // returns undefined
	person.getName() // returns 'lisi'

	var person2 = cat('张华');
	person2.getName() // returns '张华'

这里的name即是私有成员，通过person，和person2两个对象实例说明可以保存状态

从上面两个例子我们也可以看出闭包的另一个好处，通过保存状态或将定义私有成员，可以实现避免全局变量污染的好处。


我们知道在javascript中避免全局变量污染的一般方法:

1.定义全局命名空间

2.使用一个立即执行函数 

[githubGist的代码实例参考](https://gist.github.com/hallettj/64478)

[stackflow的优秀解答](https://stackoverflow.com/questions/8862665/what-does-it-mean-global-namespace-would-be-polluted)

### 闭包与立即执行函数

闭包帮我们解决了我们要全局的使用，又不想全局所带来的污染的问题，使用闭包你可以像工厂方法一样创建多个不同的对象。但在实际的开发过程中我们又会遇到这样的问题，希望对象只有一份，也就是我们常说的单例模式，在javascript中函数没法私有化，所以转个思路我们可以让这个工厂方法不能多次调用，而这样的函数就是匿名函数；而且只能调用一次，就是在声明函数的时候立即执行。

IIFE

### 文章推荐

[lucy’s blog about closure](http://lucybain.com/blog/2014/closures/)

[by rlynjb about closure](https://medium.com/@rlynjb/js-interview-question-what-is-a-closure-and-how-why-would-you-use-one-b6fd45ea95f6)

[我们面试中在被问到闭包这个问题是要注意的几点](https://github.com/lvzhenbang/article/blob/master/git/closure-translate.md)

[闭包的延伸，让面试变得so easy](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-closure-b2f0d2152b36)
