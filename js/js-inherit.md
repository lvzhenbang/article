# JavaScript几种常见的继承方式及优缺点

读了《JavaScript高级程序设计》有感，让我对函数的继承有了些许感受。

## 常见的继承方式

1. 原型链

```
var Robot = function() {}

Robot.prototype.say = function() {
	console.log('My name is ' + this.name + '.')
}

var robotDancer = function(name) {
	this.name = name
}

robotDancer.prototype = new Robot()

robotDancer.prototype.dance = function() {
	console.log('I can dance.')
}

var dancerA = new robotDancer('A')
dancerA.say() // My name is A.
dancerA.dance() // I can dance.
```

基本实现：原型链的特点是每个实例都有一个指向原型对象内部的指针，所以我们将一个类的实例赋值给一个类的原型，这样就可以实现原型链的继承；而原型对象也有一个指向构造函数的指针，所以我们就可以查找父类构造函数内的属性。

注意：因为属性name是所有的Robot的示例对象共有的属性，所以将其放在父类构造函数这样比较合理。
但是原型链的继承是父类的属性和方法的共享，这样子类继承了父类的属性后，会出现子类修改继承的属性后，父类的属性也放生改变，所以在开发只将可读的共享属性放在父类中。

2. 借用构造函数

```
var Robot = function(name) {
	this.name  = name
	this.say = function() {
		console.log('My name is ' + this.name + '.')
	}
}

Robot.prototype.hello = function() {
	console.log('I am a intelligeng robot.')
}

var robotDancer = function() {
	Robot.apply(this, arguments)
	this.dance = function() {
		console.log('I can dance.')
	}
}

var dancerA = new robotDancer('A')
dancerA.say() // My name is A.
dancerA.dance() // I can dance.
dancerA.hello() // Uncaught TypeError: dancerA.hello is not a function

var dancerB = new robotDancer('B')
dancerB.say() // My name is B.
dancerB.dance() // I can dance.
dancerB.hello() // Uncaught TypeError: dancerB.hello is not a function
```

基本实现：可以通过apply()和call()改变构造函数内部的this指向对象

注：该实现保证了子类对父类的属性的继承，也即是父类构造函数this的挂载的属性和方法，而不能实现对父类原型链上的属性和方法的继承(因为开发中我们往往需要实现部分属性和方法的共享)，而借用构造函数不能实现父类的方法和属性共享（），只能实现方法和属性的私有。

3. 组合继承

就是将原型链和借用构造函数组合到一起。

```
var Robot = function(name) {
	this.name = name
	console.log('Robot instantiation.')
}

Robot.prototype.say = function() {
	console.log('My name is ' + this.name + '.')
}

var robotDancer = function() {
	Robot.apply(this, arguments)
}

robotDancer.prototype = new Robot()
robotDancer.prototype.constructor = robotDancer
robotDancer.prototype.dance = function() {
	console.log('I can dance.')
}

var dancerA = new robotDancer('A')
dancerA.say() // My name is A.
dancerA.dance() // I can dance.

var dancerB = new robotDancer('B')
dancerB.say() // My name is B.
dancerB.dance() // I can dance.
```

注：通过在父类构造函数中加入 `console.log('Robot instantiation.')` 打印执行代码，发现它被在执行了三次调用。通过调试发现第一次是在 `robotDancer.prototype = new Robot()` ，第二次是在 `dancerA = new robotDancer('A')` ，第三次是在 `dancerB = new robotDancer('B')` ，第一次的实例化没有必要。

调整一：

熟悉原型链的话，我们知道实例的对象有一个指针指向原型对象的内部，因此我们可以用原型的对象代替实例化后的对象。所以可以将代码修改如下：

```
var Robot = function(name) {
	this.name = name
	console.log('Robot instantiation.')
}

Robot.prototype.say = function() {
	console.log('My name is ' + this.name + '.')
}

var robotDancer = function() {
	Robot.apply(this, arguments)
}

robotDancer.prototype = Robot.prototype
robotDancer.prototype.constructor = robotDancer
robotDancer.prototype.dance = function() {
	console.log('I can dance.')
}

var dancerA = new robotDancer('A')
dancerA.say() // My name is A.
dancerA.dance() // I can dance.

var dancerB = new robotDancer('B')
dancerB.say() // My name is B.
dancerB.dance() // I can dance.
```

注：诚如我们前面说的实例有一个指向原型对象的指针，原型对象有一个指向构造函数的指针。所以为了避免实例对象的构造函数自动指向父类的构造函数，我们要 `robotDancer.prototype.constructor = robotDancer` 指明构造函数的指向子类。

4. 原型式继承

基本实现：通过已有的对象来创建新的对象，然后通过原型链来实现继承。

```
var robotA = {
	name: 'robotA',
}

var F = function() {}
F.prototype = robotA

var robotB = new F()
robotB.name = 'robotB'
```

很明显这跟原型链继承很像。原型链是通过实例化一个类，然后通过原型链实现继承；而原型式继承是直接通过一个对象的复制，然后通过原型链来实现继承。

那么，原型式继承如何实现对象的克隆的呢？这里先说一下ES5中的` Object.create() `方法如何实现对象的克隆，我们在该方法中传入一个对象，就得到一个该对象的副本；如果传入一个空参数，就得到一个空对象。

注：[` MDN Object.createJ() `](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create)，这里介绍了它如何使用，以及如何用它实现类的继承。

所以，上面的代码可修改如下：

```
var robotA = {
	name: 'robotA',
}

function cloneObj = function(obj) {
	var F = function() {}
	F.prototype = obj

	ruturn new F()
}

va robotB = cloneObj(robotA)
robotB.name = 'robotB'
```

注：由于原型式继承脱胎于原型链继承，所以原型链继承有的缺点它都有。

原型式继承只是实现了对对象的拷贝，下面介绍一个对原型继承的加强版——寄生式继承。

5. 寄生式继承

基本实现：所谓的加强就是在原型式继承的基础上给克隆的对象添加私有属性和方法。它实现了对继承过程的封装。

代码实现如下：

```
var robotA = {
	name: 'robotA',
}

function cloneObj(obj) {
	var cloneObj = Object.create(obj)

	cloneObj.dance = function() {
		console.log('I am dancing.')
	}

	return cloneObj
}

var robotB = cloneObj(robotA)
robotB.dance()
crobotB.dance() // 'I am dancing.'
```

注：它拥有原型式继承所拥有的缺点，也即是原型链继承的所有缺点。

6. 寄生组合式继承

基本实现：寄生组合式继承很明显就是寄生继承和组合是继承的结合体。

```
var Robot = function(name) {
	this.name = name
	console.log('Robot instantiation.')
}

Robot.prototype.say = function() {
	console.log('My name is ' + this.name + '.')
}

var robotDancer = function() {
	Robot.apply(this, arguments)
}

function inherit(RObot, robotDancer) {
	robotDancer.prototype = Object.create(Robot.prototype)
	robotDancer.prototype.constructor = robotDancer
}

inherit(Robot, robotDancer)


robotDancer.prototype.dance = function() {
	console.log('I can dance.')
}

var dancerA = new robotDancer('A')
dancerA.say() // My name is A.
dancerA.dance() // I can dance.

var dancerB = new robotDancer('B')
dancerB.say() // My name is B.
dancerB.dance() // I can dance.
```
