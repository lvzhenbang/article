## constructor设计模式

在经典的面向对象编程语言中，constructor是一个特殊的方法，被用来初始化一个新建的对象，一旦该对象被分配内存话。

在JavaScript中几乎所有的东西都是一个对象，然而常常引起我们兴趣的是对象的constructor。

对象的构造函数用于创建特定类型的对象（既要准备使用对象，也要接收参数），构造函数在创建对象时可以用来设置成员属性和方法的值。

### 创建对象

在JavaScript中创建对象的三种通用方法如下：

	// 每种方法将创建一个空对象

	var newObject = {}

	// or

	var newObject = Object.create( Object.prototype )

	// or

	var newObject = new Object();

最后一个方法实现对象的创建可以传入指定的值，如果没有传递，将创建一个空对象，然后返回它。
	
	传入String，返回String，类似于new String();
	传入Number，返回Number，类似于new Number();
	传入Object，返回Object，相当于没传

为对象指定键和值有如下四种方法：

兼容ES3的方法

	// 1. `.`(Dot) 语法
	// 设置属性
	newObject.someKey = 'Hello world';
	// 获取属性
	var value = newObject.somekey;

	
	// 2. '[]'方括号语法
	// 设置属性
	newObject["someKey"] = "Hello World";
	// 获取属性
	var value = newObject["someKey"];

兼容ES5的方法（要了解更多的信息，请转[ http://kangax.github.com/es5-compat-table/]( http://kangax.github.com/es5-compat-table/)）

	// 3. Object.defineProperty
	// 设置属性
	Object.defineProperty(newObject, "someKey", {
		value: "Hello World",
		writable: true,
		enumerable: true,
		configurable: true
	})
	// 获取属性的方法，用1，2中方法即可

	// 详细了解Object.defineProperty的使用可参考[https://segmentfault.com/a/1190000007434923](https://segmentfault.com/a/1190000007434923)

	// 4. Object.defineProperties
	// 设置属性

	Object.defineProperties(newObject, {
		"someKey": {
			value: "Hello World",
			writable: true
		},
		"anotherKey": {
			value: "Ha ha",
			writable: true
		}
	})

	// 获取属性的方法，用1，2中方法即可

### 对象的继承

前面我们说到几种的对象的创建方式，在三种对象的创建中，第一种方式最简单，效率更高，其他两种内部都用了对象的继承,如果没有传入任何参数，实现和第一种相似。下面试第二三种创建对象的实例：

	// 第二种
	var obj = {};
	obj.a=1;
	obj.a -> 1
	var obj2 = Object.create(obj)
	obj2.a -> 1

	// 第三种
	var obj = {};
	obj.a = 1;
	obj.a -> 1
	var obj2  = new Object(obj);
	obj2.a -> 1

所以只是要创建一个空对象，第一种方法更好，要实现对象的继承用第二三种方法，第二三种方法的区别在于前者是ES3，后者是ES5。

	// Object.create 的内部实现如下：
	Object.create = function(o) {
        function F(){}
        F.prototype=o;
        return new F();
    }

### constructor 基础

众所周知，JavaScript一门不支持 `class` 这个概念，但是它支持对象的构造函数，通过关键字 `new` 我们想要一个构造函数创建一个对象和它用函数定义的成员。

在constructor的内部，`this` 关键字是被创建的新对象的引用。重温对象的创建过程，一个基础的 constructor 如下所示：
	
	function Car( model, year, miles ) {
 
	  this.model = model;
	  this.year = year;
	  this.miles = miles;
	 
	  this.toString = function () {
		return this.model + " has done " + this.miles + " miles";
	  };
	}
	 
	// 用法:
	 
	// 创建一个car的实例 
	var civic = new Car( "Honda Civic", 2009, 20000 );
	var mondeo = new Car( "Ford Mondeo", 2010, 5000 );
	 
	// 结果输出
	console.log( civic.toString() ); -> "Honda Civic has done 20000 miles"
	console.log( mondeo.toString() ); -> "Ford Mondeo has done 5000 miles"·

如果现在就把它定义为一个设计模式相信，很多人对不会用它，因为它存在着一些问题。其中一个就是继承性，另一个是每创建一个对象实例，`toString()` 方法都要被重新定义，理想的实现方式是要实现 `toString()` 在不同的实例类型之间共享。

#### constructor 和 prototype

在JavaScript中，`Function` 像其他对象一样，有一个 `prototype` 。当我们用contructor创建一个对象，所有constructor的 prototype上的属性都将被新对象继承。所以上面的例子我们可以修改如下：

	function Car( model, year, miles ) {
		this.model = model;
		this.year = year;
		this.miles = miles;
	}

	Car.prototype.toString = function () {
		return this.model + " has done " + this.miles + " miles";
	};
	 
	// 用法:
	 
	// 创建一个car的实例 
	var civic = new Car( "Honda Civic", 2009, 20000 );
	var mondeo = new Car( "Ford Mondeo", 2010, 5000 );
	 
	// 结果输出
	console.log( civic.toString() ); -> "Honda Civic has done 20000 miles"
	console.log( mondeo.toString() ); -> "Ford Mondeo has done 5000 miles"·

这样，`toString()` 将实现在不同的对象实例间的共享。

其它设计模式相关文章请转[‘大处着眼，小处着手’——设计模式系列](https://github.com/lvzhenbang/article/blob/master/design-pattern/introduce.md)