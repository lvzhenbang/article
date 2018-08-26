## 装饰者设计模式

装饰者设计模式又被称作包装器设计模式，包装器很好理解就是对所实现的对象添加方法进行的包装处理。

在JavaScript中我们经常会遇到用原型链来模拟类的继承。而装饰者模式就是解决继承模式的不灵活的特性。

### 继承是什么

继承就是让一个引用类型继承另一个引用类型的属性和方法。

### 继承的特性

* 由于父类和子类间的强耦合性，使得我们给父类添加功能，往往会影响子类；
* 同时在复用时，可能创造大量的子类。

### 继承的几种常见方式


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

2. 构造函数

### 装饰者模式的实现
