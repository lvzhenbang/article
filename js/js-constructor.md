## 构造函数

### 一般构造函数

```
var Robot = function(name) {
	this.name = name
} 

var robert = new robert('robert')

robert.name // "robert"
```

### 寄生构造函数

创建一个用来封装创建对象的函数，然后再返回新创建的对象。这个函数看起来就像是一个构造函数一样。

```
var Robot = function(name) {
	var obj = new Object()
	obj.name = name
	obj.say = function() {
		console.log("My name is " + name + ".")
	}

	return obj
}
var robotA = new Robot('lin')
robotA.say() // "My name is lin."
```

返回的对象和构造函数本身没有什么关系，也即是说该对象与在构造函数外创建的对象没有什么区别。

这种构造函数可以参考《JavaScript高级程序设计》中的例子，在不修改构造函数Array的情况下为数组添加新的方法，代码如下：

```
var SpecialArray = function() {
	var arr = new Array()

	arr.push.apply(arr, arguments)

	arr.toPipeString = function() {
		return this.join('|')
	}

	return arr
}

var colors = new SpecialArray('red', 'blue', 'green')

console.log(colors.toPipeString()) // "red|blue|green"
```


### 稳妥构造函数

稳妥对象就是没有公共属性，其方法也不引用this的对象（它是由道格拉斯·克罗克福德提出的）。

稳妥构造函数和寄生构造函数很相似，但是它与寄生构造函数有两点不同：

* 新创建的对象的实例方法不引用this
* 不使用new操作符调用构造函数

```
var Robot = function(name) {
	var obj = new Object()
	
	// 添加私有方法
	obj.say = function() {
		console.log("My name is " + name + ".")
	}

	return obj
}

var robotA = Robot('lin')
robotA.say() // "My name is lin."
```

这样的构造函数创建的对象除了通过它提供的方法外没有别的方式访问其传入的原始数据，但可以给它添加方法或数据成员。