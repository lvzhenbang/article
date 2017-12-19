## 立即执行函数

#### 问题

	function() { console.log('hello world!')} ()

1.上面的IIFE立即执行函数代码是如何执行的?
2.能否输出正确的结果?
3.如果不能,请说明输出的结果？
4.那么如何修改，将输出所要的正确结果?

在面试的时候我们常常会遇到考官这样层层递进的问我们问题。

### 代码的执行

在我们平时的开发过程中我们常常用的IIFE代码格式如下：
	
	(function() {
		// code
	})()

所以，一般面试中遇到这样的问题，我们就要留个心眼了。废话不多说，直奔主题。

javascript中以function关键字开头的语句会被浏览器引擎解析为函数声明，而函数声明是不允许直接运行的。

### 不能输出正确的结果

首先会进行function() {console.log('hello world')}的匿名函数声明，然后进行表达式的运算，运算符为()
但该实例代码中没有表达式，所以会报‘Uncaught SyntaxError: Unexpected token (’的错误

如果我们将代码作如下修改：

	function b() { console.log('hello world!')} (1+1)

在控制台会显示如下执行结果：

![执行结果](https://github.com/lvzhenbang/article/blob/master/img/js/iife.png)

### 修改成为真正的IIFE

由于函数表达式能够在解析器下直接运行，因此解决办法就来了以运算符开头，这样上述函数就成为了IIFE，修改样例如下：

	+function() { console.log('hello world!')} ()

另一种，将函数体编程表达式代码如下：
	
	(function() { console.log('hello world!')}) ()

第二种写法使我们常见的写法，第一种加运算符的不建议

### IIFE 与闭包

未完待续...

### 如何写一个标准的IIFE 

[这里推荐一篇关于IIFE的文章](https://segmentfault.com/a/1190000003985390)