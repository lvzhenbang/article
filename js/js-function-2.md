## JS开发中函数知识点梳理(二）

作为一个Jser，不光要会用js，还要明白它的运行原理，不然就会一直停留在表面。

函数在JavaScript中被称作`第一等公民`，这个第一等公民是什么鬼？看看[知乎](https://www.zhihu.com/question/67652709)上是怎么回答的。就像我的引路人刚开始跟我说的要想学好一门语言，就要先掌握好一门外语（英语）一样，因为这些计算机编程语言或解释器语言基本都是源于老外开发，所以要想学到原汁原味的东西，查看英文文档是必不可少的。

英文原文中本来是 `first-class object` ，而翻译成 `第一类公民` 其实就是一种比喻。从这里可以知道两点：

* 函数本质上也是对象，
* 可以用函数实现其它的任何对象

### 函数的用法

1. 可以动态的创建函数 （new Function()）

这种方式不常用，也不推荐。具体原因是（来自于JavaScript高级程序设计）：这种语法会导致解析两次代码，从而影响性能。

2. 可以将函数赋值给变量（函数表达式）

3. 可以将函数最为一个参数传递给另一个函数（回调函数）

4. 函数可以包含自己的属性和方法（构造函数）

5. 将一个函数作为另一个函数的返回值

对象数组的排序，代码如下：

```
function compare(prop) {
	return fucntion(obj1, obj2) {
		var v1 = obj1[prop],
			v2 = obj2[prop]

		return v1 > v2 ? 1 : v1 < v2 ? -1 : 0
	}
}

var arr = [{
	name: 'li',
	age: 18
	}, {
	name: 'an',
	age: 19
	}, {
	name: 'tian',
	age: 18
	}]

arr.sort(compare('name'))
```

### 函数与对象之间的关系

通过上面的描述，不管之前知道不知道，但是现在应该知道，我们可以通过函数来创建对象。

代码说明：

```
function Robot(name) {
	this.name = name
}

var robert = new Robot('robert')

robert.__proto__.constructor // ƒ Robot(name) {this.name = name}

roboert.__proto__.constructor.__proto__.constructor // ƒ Function() { [native code] }

robert.__proto__.__proto__.constructor // ƒ Object() { [native code] }

robert.__proto__.__proto__.constructor.__proto__.constructor // ƒ Function() { [native code] }

robert.__proto__.__proto__.__proto__ // null
```

通过原型链，我们可以知道我们的实例对象源于谁。如上面的例子，我们创建了构造函数 Robot，用它实例化了一个robert对象，所以robert对象源自于构造函数Robot，而构造函数Robot的原型通过打印值，我们知道它源自于对象Function；接着看，通过原型链继承我们可以知道，Robot继承自对象Object，而Object的构造函数则源自于Function；而顺着原型链我们查找Object的原型的对象，会得到一个空值。所以，通过上述的结果，我们发现在js中不管我们是用构造函数创建的对象还是用js本身提供的数据类型创建的对象都源自于Function。

在js中创建对象的基本方式大致分为四类：

* 构造函数 (如：一般构造函数，寄生构造函数，稳妥构造函数等)
* 包装器（如：new Number()/new Object()等等）
* 对象字面量 （如：var obj = {name: 'robert', age: 18}）
* 原型

然后，就是根据需要使用上面的基本方式的随机组合。

### Function对象的属性

Function既然是个对象，那么它就可以拥有自己的属性。这个我们可以在浏览器控制台输入 `函数名.` 后，浏览器就可以自动提示函数的属性。而我们常用的式函数的内部属性，我们常见的就是 `arguments` 和 `this`。前者是一个包含函数传入的参数伪数组，后者指向函数对象本身。同时我们也注意到了`arguments`对象包含一个属性 `callee` ，它是一个指针，指向包含 `arguments` 属性的函数。它和 `this` 的区别就是arguments.callee()可以代表函数本身，而 `this` 就是函数执行环境的对象。

使用arguments.callee()可以解除函数体内代码和函数名的耦合状态。

参考代码如下：

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

通过函数fic4和fic5的比较我们可以的出上面的结论。

* name: 函数的名字
* length: 函数传入参数的个数

### Function对象的方法

我们常见的是 `apply/call` ， apply方法接收两个参数，第一个都是在其中运行的函数作用域，第二个参数为一个参数数组。

在ES5中还有一个方法bind也可以改变函数运行时内部的作用域，它有一个参数，该参数就是函数内部this要绑定的对象。
