## 单例设计模式 (The Singleton Pattern)

### 什么是单例模式

单例模式是很知名的，因为它将类的实例化限制为单个对象。经典地单例模式通过一个方法创建一个类来实现，该方法可以创建一个不存在的类的新实例。在实例存在的情况下，它只返回该对象的引用。

### 注意事项

与静态类(或对象)不同的是，我们可以延迟单例的初始化，通常是因为它们需要一些在初始化期间可能无法获得的信息。它们不提供一种代码，而这些代码不知道前面提到的代码可以轻松地检索它们。这是因为它不是单元素返回的对象或“类”，而是一个结构。就好像闭包变量实际上并没有闭包，提供闭包的函数范围是闭包。

### 单例的示例

在JavaScript中，Singletons作为共享资源命名空间，它将实现代码从全局命名空间中分离出来，从而为函数提供单一的访问点。

下面是一个单例实现:

```
var mySingleton = (function () {
	// 实例存储单例的引用
	var instance;
	function init() {
		// 单例
		// 私有方法和变量
		function privateMethod(){
				console.log( "I am private" );
		}
		var privateVariable = "Im also private";
		var privateRandomNumber = Math.random();
		return {
			// 公共方法和变量
			publicMethod: function () {
				console.log( "The public can see me!" );
			},
			publicProperty: "I am also public",
			getRandomNumber: function() {
				return privateRandomNumber;
			}
		};
	};
	
	return {
		// 如果单例的实例存在就获得它，不存在就创建一个
		getInstance: function () {
			if ( !instance ) {
				instance = init();
			}
			return instance;
		}
	};
})();
	
var myBadSingleton = (function () {
	// 实例存储一个单例的引用
	var instance;
	function init() {
		// 单例
		var privateRandomNumber = Math.random();
		return {
			getRandomNumber: function() {
				return privateRandomNumber;
			}
		};
	};
	
	return {
		// 创建一个单例实例
		getInstance: function () {
			instance = init();
			return instance;
		}
	};
})();
	
// 用法:
var singleA = mySingleton.getInstance();
var singleB = mySingleton.getInstance();
console.log( singleA.getRandomNumber() === singleB.getRandomNumber() ); // true
	
var badSingleA = myBadSingleton.getInstance();
var badSingleB = myBadSingleton.getInstance();
console.log( badSingleA.getRandomNumber() !== badSingleB.getRandomNumber() ); // true
```

注意: 当我们用随机数来处理时，有一个可能性两个数字是一样的，无论多么不可能。上述示例在其他情况下仍然有效.

通常使单例能全局的访问实例（通常通过MySingleton.getInstance()），我们无法（至少在静态语言中）调用新的MySingleton()，然而这在JavaScript中是可能的，因为JavaScript可以进行函数作为一个参数进行传参。

在GoF这本书中，单例模式适用性描述如下:

1. 必须有一个类的一个实例，并且必须从一个公共的访问点访问实例；
2. 当唯一的实例应该通过子类化扩展时，实例应该能够使用扩展实例而不修改其代码。

第二点我们可以用一个示例说明，代码如下:

```
mySingleton.getInstance = function(){
	if ( this._instance == null ) {
		if ( isFoo() ) {
				this._instance = new FooSingleton();
		} else {
				this._instance = new BasicSingleton();
		}
	}
	return this._instance;
};
``` 

在这里，getInstance变得有点像工厂方法，但在我们的代码中不需要更新每一点。上面的FooSingleton将是BasicSingleton的一个子类.

为什么说延迟执行对单例很重要:

在c++中，它可以从动态初始化顺序的不可预测性中分离出来，并将控制权返回给程序员。

重要的是要注意一个类(对象)和一个单例的静态实例之间的区别:虽然单例可以作为一个静态实例来实现，但它也可以被惰性地构建，而不需要资源和内存，直到实际需要。

如果我们有一个可以直接初始化的静态对象，我们需要确保代码总是以相同的顺序执行(例如：在初始化时objCar需要 objWheel，而当你有大量的源文件时，它就不会缩放。

单例和静态对象都是有用的，但是它们不应该被过度使用,同样的我们不应该过度使用其他模式。

在实践中，当需要一个对象来协调系统中的其他对象时，单例模式是有用的。这里有一个例子，在这个背景下使用的模式是:

```
var SingletonTester = (function () {
	// options: 一个包含单例配置项的对象
	// 例如： var options = { name: "test", pointX: 5};
	function Singleton( options ) {
		// 如果没有提供选项，则将选项设置为提供的选项或空对象
		options = options || {};
		// 为我们的单里设置一些属性
		this.name = "SingletonTester";
		this.pointX = options.pointX || 6;
		this.pointY = options.pointY || 10;
	}
	
	// 实例
	var instance;
	// 静态变量和方法的仿真
	var _static = {
		name: "SingletonTester",
		// 获取实例的方法，它返回一个单例对象的实例
		getInstance: function( options ) {
			if( instance === undefined ) {
				instance = new Singleton( options );
			}
			return instance;
		}
	};
	return _static;
})();
	
var singletonTest = SingletonTester.getInstance({
	pointX: 5
});
	
// 如果验证正确输出 pointX
// Outputs: 5
console.log( singletonTest.pointX );
```

虽然，Singleton具有很好的用途，但通常当我们在JavaScript中发现自己需要它时，它是一个标志表明我们的项目设计可能存在着问题，这时需要重新评估设计。

### 总结

应用中的模块要么是紧密耦合的，要么逻辑在代码库中各个部分过度分散。如果在隐藏的依赖关系、创建多个实例、存储依赖等方面遇到困难，那么单例对象可能更难测试，不适合使用。


### 参考资料

[http://www.ibm.com/developerworks/webservices/library/co-single/index.html](http://www.ibm.com/developerworks/webservices/library/co-single/index.html)

[http://misko.hevery.com/2008/10/21/dependency-injection-myth-reference-passing/](http://misko.hevery.com/2008/10/21/dependency-injection-myth-reference-passing/)
