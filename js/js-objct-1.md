## 创建对象的方法

读《JavaScript高级程序设计》所得。

工厂模式——》构造函数模式——》原型模式——》构造函数和原型模式的组合——》动态原型模式——》寄生构造函数模式——》稳妥构造函数模式

### 工厂模式

```
function createPerson(name, age) {
	var o = new Object()
	o.name = name
	o.age = age
	o.sayName = function() {
		console.log(this.name)
	}
}

var robert = createPerson('robert', 20)
```

实现了创建多个相似对象的问题。

缺点：没有解决对象的识别问题。

### 构造函数模式

```
function Person(name, age) {
	this.name = name
	this.age = age
	this.sayName = function() {
		console.log(this.name)
	}
}


var robert = new Person('robert', 20)
```

可以通过构造函数来识别新创建对象的类型。

缺点：每个实例上的方法都会创建一遍。因为在JavaScript中函数本质也是对象，所以这样就造成了相似的操作。这样的方法不如直接用一个全局的不同函数来代替，但是在全局作用域下的函数的引用实际上还是被某个对象调用，而且更多的相似的全局方法会造成使用上的麻烦。

### 原型模式

```
function Person() {}

Person.prototype.name = 'robert'
Person.prototype.age = '10'
Person.prototype.sayName = function() {
	console.log(this.name)
}

var robert = new Person()

robert.sayName() // "robert" 
```

原型模式创建的对象实例共享方法和属性。可以通过Object.hasOwnProperty()来判断属性是实例本身，还是来自于原型。

缺点：1.它省略了为构造函数传递初始化参数之一环节，所以所有的实例在默认的情况下将获得相同的属性值；2.众所周知，在原型链中属性是共享的，这对于非引用类型的属性来说，可以通过给实例添加相同的属性覆盖原型链上的属性，但是对于引用类型的属性来说，你修改了任何一个实例的引用类型属性，将会影响其它实例的该引用类型属性。

### 构造函数和原型模式的组合

创建自定义类型对象最常见的方式就是它。构造函数用于定义实例属性，而原型模式用于定义方法和共享属性。这样每个实例都有自己的一份实例属性的副本，同时有共享方法的使用，这样就大大地节省了内存。同时也保持向构造函数 传参。

```
var Robot = function(name) {
	this.name = name
}

Robot.prototype = {
	say: function() {
		console.log('My name is ' + this.name + '.')
	},
	dance: function() {
		console.log('I can dance.')
	}
}

var robert = new Robot('robert')

robert.say() // "My name is robert."
robert.dance() // "I can dance."
```

### 动态原型模式

为了避免开发人员看到独立的构造函数和原型时的困惑，这种模式将所有的信息都封装在构造函数，
通过检查某个应该存在的方法是否有效，来决定是否需要初始化原型。

```
var Robot = function(name) {
	this.name = name

	if(typeof this.say !== 'function') {
		Robot.prototype.say = functon() {
			console.log('My name is ' + this.name + '.')
		}
	}
}

var robert = new Robot('robert')

robert.say() // "My name is robert."
```

注意：构造函数中的say()方法，在初次调用构造函数时才会执行，并且只有在该方法不存在的情况下，才会将它添加到原型中。

下面两种是特殊的创建对象的方式。

### 寄生构造函数模式

这种模式的基本思想就是创建一个函数，该函数封装了创建对象的过程，然后返回新创建的对象。这个函数外表看起来像构造函数。通常在前几重创建对象的模式不适用的情况下使用。

```
var Robot = function(name) {
	var obj = new Object()
	obj.name = name

	obj.say = function() {
		console.log('My name is ' + this.name + '.')
	}

	return obj
}

var robert = new Robot('robert')

robert.say() // "My name is robert."
```

### 稳妥构造函数模式

稳妥构造函数模式是道格拉斯·克罗克福德发明的稳妥对象(稳妥对象就是指没有公共属性，而且其方法也不引用this的对象)衍生而来。稳妥对象适合在一些安全环境中（禁止使用this和new关键字），防止数据被其他程序使用。

它和寄生构造函数很像，不同点在于：

* 新创建对象的实例方法不使用this
* 不使用new操作符调用构造函数

```
var Robot = function(name) {
	var obj = new Object()

	obj.say = function() {
		console.log('My name is ' + name + '.')
	}

	return obj
}

var robert = Robot('robert')

robert.say() // "My name is robert."
```

