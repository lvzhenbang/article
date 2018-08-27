## JS开发中函数知识点梳理(三）

这篇文章要介绍的内容是函数表达，因为我个人比较喜欢使用函数表达式定义函数，所以就对它做了一些研究和整理。其实，说到函数表达式，就不得不说到定义函数的另一种方式——函数声明，它们虽然相似，但是又有一些区别，那么它们的区别是什么呢，这个问题会在本文中单独说明，下面进入正文。

### 函数声明

常见结构如下：

```
function functionName(arg0, arg1,[...]) {
	// 函数体
}
```

由一个关键字 `function` 开头，后面紧跟函数名，然后是函数的参数（如：(arg0, arg1,[...])），最后是函数体（如：‘{ // 函数体}’）

在JavaScript中，函数声明有一个特性是函数声明的提升，也就是说在代码执行前会先读取函数声明，因此，我们把函数声明放在调用它的语句的前后皆可。

### 函数的调用方式

* 作为函数
* 作为方法
* 作为构造函数
* 通过函数的call()和apply()间接的调用

### 函数表达式的分类

1. 匿名函数表达式

```
var func = function() {
	console.log('I am an anonymous function expression.')
}
```

2. 具名函数表达式

```
var func = function functionname() {
	console.log('I am a named function expression.')
}
```

3. 自调用函数表达式

```
(function() {
	console.log('I am a self-call function expression.')
})()
```

### 函数表达式

常见结构如下：

```
var functionName = function(arg0, arg1,[...]) {
	// 函数体
}
```

这种创建一个匿名函数，然后将它赋值给变量functionName的表达式形式，就叫做函数表达式。

### 函数表达式的用法

1. 递归

主要使用 `arguments.callee` 来接偶函数名和函数体的耦合状态。

```
function fic(n) {
	return n <=1 ? 1 : n * arguments.callee(n-1)
}

function fic2(n) {
	return n <= 1 ? 1 : n * fic(n-1)
}

function fic3() {
	return 0
}

fic(5)  // 120
fic2(5) // 120

var fic4 = fic
var fic5 = fic2
fic = fic3
fic2 = fic3

fic(5)  // 0
fic2(5) // 0
fic4(5) // 120
fic5(5) // 0
```

但是使用 `arguments.callee` 在严格模式下存在着问题。因此，我们会使用命名函数表达式来解决这个问题。

修改fic函数表达式代码如下：

```
fic = (function f(n) {
	return n <= 1 ? 1 : n * f(n-1)
})
```

2. 闭包

我们知道，函数表达式是将匿名函数赋值给一个变量，作为变量的值，所以，匿名函数也可以作为return的返回值。

这和闭包的使用是不是很相似。关于闭包的知识点可以参考这篇文章[https://github.com/lvzhenbang/article/blob/master/js/closure.md](https://github.com/lvzhenbang/article/blob/master/js/closure.md)

### 函数表达式 VS 函数声明

建议先看一下[Function Declarations vs. Function Expressions](https://javascriptweblog.wordpress.com/2010/07/06/function-declarations-vs-function-expressions/)这一篇文章。

这篇文章开始先引用了几个示例来说明，使用函数表达式和函数声明的区别。

1. 函数声明有一个规则我们应该比较清楚就是函数声明的提升。声明的函数不管在我们调用函数语句之前，还是之后，只要函数声明存在，调用函数就可以执行成功。

2. 变量的赋值语句，在执行的过程中也存在着变量声明的提升。但是要牢记变量的提升是变量定义的提升，而不是变量赋值的提升，变量赋值还在原来的位置，另外还要记住的是变量定义的提升的同时还会给变量赋值一个初始值为undefined。

3. 函数表达式和变量的赋值表达式基本类似，只不过函数表达式所赋的值为一个函数。

另外，需要弄明白的一点是JavaScript这门面向对象语言没有重载（拥有相同函数名，但函数的参数不同）这个概念，但是如果后写的代码和前面写的代码定义重复，后面的代码将会覆盖前面代码。

