## JavaScript 知识点

### 对象

说到JavaScript，我们就总是会提到对象，说到对象我们就会提到原型继承，这是我们接触JavaScript时经常会遇到的，尤其是在面试中我们也是会经常遇到。一般知道对象，理解原型看起来是足够够了，但是有一些小细节的问题，我想我们还是要注意的，下面我们就来说说。

### 对象的常见创建方法

#### new Object()

实现对象的继承，继承自Object，这种方法现在不常用

```
let obj = new Object()
```

#### 字面量对象 

定义一个非继承的对象

```
let obj = {a:1, b:2, c:3}
```

#### objet.create() 

实现一个可以继承自任意对象的对象

```
let obj = Object.create({a:1, b:2, c:3});
let obj2 = Object.create(null); // 和 {} 一样
```

#### 构造函数 

它是实现对象实例的基础

```
function Func(name) {
	this.name = name;
}
let obj = new Func('lzb')
```

#### 构造函数 + prototype 

实现一个自定义的可方法和属性共享的对象

```
function Func() {} 
Funct.prototype.name = 'func';
let obj = new Func();
```

#### ES6中的 class 

模仿其他语言的类的继承，底层实现还是原型继承

```
class Func {
	constructor(name) {
		this.name = name;
	}

	getName() {
		console.log(this.name)
	}
}

let obj = new Func('lzb');
obj.getname(); // lzb
```

#### 单例模式 

一个类只有一个实例

```
let singleton = new function() {
	this.name = 'lzb'
}
```

创建对象的核心还是构造函数和prototype的结合，根据不同的用途和使用场景而形成了其它的实现方法。

#### 对象和属性

在JavaScript中不是只有我们用 `new Object()` , `Object.create()`， 字面量创建的对象才是对象，其实在JavaScript中所有的变量都可以当作对象使用，除了 `null`，`undefined`。

我们知道在由于原型继承的关系，对象可以使用对象的方法，如下示例。

```
// Boolean
false.toString(); // "false"
// Array
[1, 2, 3, 4, 5, 6].toString(); // "1,2,3,4,5,6"
// Number
(1).toString(); // "1"
// Constructor
function Func() {};
Func.name = "func";
Func.name; // "func"
```

#### 数字的字面值（literal） 不能当作对象使用

按照正常的解析数字字面量是可以被当作对象使用的，但事实上，我们使用后就会报 `Uncaught SyntaxError: Invalid or unexpected token` 的错误，这是由于JavaScript解析器试图将 `.` 操作符解析为浮点数的一部分。

详细结果看下面用例：

```
1.toString(); //Uncaught SyntaxError: Invalid or unexpected token

// 浮点数可以正常使用
2.0.toString(); // "2"

// 解决办法用运算符先进行运算
(1).toString(); // "1"
```

### 对象属性的两种访问方法和注意事项

对JavaScript有一定了解的同学应该都知道，对象属性的两种访问方法 `.` 和 `[]` ，下面我们就以字面量对象为例来进行说明：

```
var foo = {}; // 空对象
var foo2 = { name: 'foo2' }; // 拥有一个值为 'foo2' 的自定义属性 'name' 的对象
```

#### 访问对象属性

```
var foo = { name: 'foo2' };

foo.name; // 
foo['name']; //
```

#### 注意事项

但是当动态的设置属性或者属性名不是一个有效的变量名时，这两种方法会出现一些差别，`[]` 这种方法对这两种情况都适用。

```
var foo = {"1234": 'abcd', 'my name': 'foo'};

foo.1234 // Uncaught SyntaxError: Unexpected number

foo['1234'] // "abcd"
// 'my name' 属性你只能用 '[]' 方法来获取
foo['my name'] // "foo" 
```

### 删除属性

我们要明确一点删除属性的方法只有一个是 `delete` ，但是在我们的开发中我们有时往往将属性值设为 `null`，`undefined` 就说成是将属性删除，这是不对的这样做仅仅是取消了属性和值之间的关联。

我们可以通过 `Object.hasOwnProperty()` 这个方法来看一看；

```
let foo = {
	name: 'foo',
	firstChild: '1st foo',
	twoChild: '2th foo'
};

foo.name = undefined; // undefined
foo.firstChild = null; // null
delete foo.twoChild; // true
for (let key in foo) {
	console.log('key:' + key + ', value:' + foo[key]);
}
/* 
key:name, value:undefined
key:firstChild, value: null
*/
```

我们发现如果用 `undefined` 和 `null` 相当于给属性赋值，只有当用 `delete` 才是删除属性。

注：定义属性的名字时要使用一般的字符串而且是连续的字符串，属性名的定义要避免JavaScript关键字，就像我们有的公司定的对象属性的访问用 `[]` , 对象的方法用 `.` 。

### 对象与原型 (prototype)

原型虽然很强大，但使用起来困难，这被很多人诟病，很多人认为原型的继承实现起来没有类的继承简单，事实上确实如此，但是现实时JavaScript一直沿用原型继承，包括最新的ES6，ES7，虽然有了类的功能，但是类的实现基础还是原型，所以掌握原型还是很有必要的。

```
function Foo() {
	this.name = 'foo',
	this.age = '18'
};

Foo.prototype.geAge = function() {
	console.log(this.name)
};

function Bar() {};

Bar.prototype = new Foo();
Bar.prototype.constructor = Bar; // 修正Bar.prototype.constructor 指向它本身
Bar.prototype.name = 'bar';
// 实例化Bar
let myBar = new Bar();

myBar.age; // "18"

```

通过上面的例子我们知道了Bar的属性继承了Foo的属性，这也就是我们所熟知的原型继承，它有有这样的属性查找规则——当查找一个对象的属性时，如果在该对象上未找到，JavaScript会遍历它的原型链，直到找到该属性为止，如果未找到，就返回 `undefined` 。

细心的同学会发现 `Object` 是对象，而在 `Object.prototype` 中 `prototype` 是它的属性，我们能不能给 `prototype` 赋值为任意的变量，它的答案也是肯定的——不能。因为  `Object.prototype` 的类型已经决定它只能是一个对象，所以它的赋值类型也只能是对象类型。

我们知道 `Object.prototype` 也是一个对象，叫做原型对象。那么原型对象的赋值是不是跟一般的对象一样，答案是肯定的——一样。

我们都知道在原型链上查找对象的属性，是要遍历原型链的，遍历原型链就意味着要耗费时间，特别是查找原型链上不存在的属性，这样耗费的时间更多，所以定义原型对象的属性时要慎重。

### monkey patching

`monkey patching` 这种技术的出现就是为了解决不同浏览器内置类型方法的兼容性问题，它的原理就是利用原型重写了JavaScript一些内置对象的方法，从而达到兼容使用的目的。由于这种手段的实现破坏了JavaScript原有封装，很多人不认可。

### 如何判断一个属性是对象自身所有，而非来自原型继承

在JavaScript中提供了一个继承自 `Object.prototype` 的方法 `hasOwnProperty` ，它可以查找对象本身的属性，但不会查找它原型链上的属性。

```
function Foo() {
	this.name = 'foo',
	this.age = '18'
};

Foo.prototype.geAge = function() {
	console.log(this.name)
};

function Bar() {
	this.name = 'bar';
};

Bar.prototype = new Foo();
Bar.prototype.constructor = Bar; // 修正Bar.prototype.constructor 指向它本身

// 实例化Bar
let myBar = new Bar();

'name' in myBar // true
myBar.hasOwnProperty('name'); // true
myBar.hasOwnProperty('age'); // false
```


### 遍历对象的属性 (for in) vs (for of)

有些人认为， `for … in` 就是用来遍历对象的，的确是这样，但是能不能用来遍历数组？我认为数组作为特殊的对象是可以被遍历的，但是事实上我们用它来遍历数组存在一些问题，比如：index索引为字符串不能用来进行几何计算，遍历的顺序可能不是数组内部的实际顺序，遍历结果会打印出 `length` 等问题。事实证明两了点，一是数组也是对象，二是数组是特殊的对象（作为一种数据类型）。而作为一种数据类型，我么们就要使用专门处理数组类型的方法，比如：`array.forEach()` 。

那么，ES6中的 `for … of` 呢，有些人认为它就是遍历数组的，那么我就想问一问了，ES5中已经有 `array.forEach()` ，为什在ES6中要再添加一个 `for … of` ，特别是一些人写的博文就直接说它是用来遍历 数组的，我感觉这样不负责任，在ES6中 `for … of` 被用来遍历可迭代对象，而在ES5中 `for … in` 是用来遍历可枚举对象的。

#### for in

在ES6之前我们使用 `for … in` 这个循环体来遍历对象的属性，包括对象原型链上的属性。

```
let obj = {a:1, b:2, c:3}
for (let key in obj) {
	console.log(obj[key]);
}

/*
1
2
3
 */
```

```
// 自定义属性是否可枚举
function customObjectEnumerable(obj, prop, value, flag) {
	Object.defineProperty(obj, prop, {
		configurable: true,
		value: value,
		enumerable: flag,
		writable: true
	});
};

var obj = {};
customObjectEnumerable(obj, a, 1, false);
customObjectEnumerable(obj, b, 2, false);
for(let key in obj) {console.log(obj[key])}
// undefined

var obj2 = {};
customObjectEnumerable(obj2,'a', 1, true);
customObjectEnumerable(obj2, 'b', 2, true);
for(let key in obj2) {console.log(obj2[key])}
/*
1
2
 */

```

#### Object.keys()

而我们们在ES5中遍历对象属性的另一个方法是 `Object.keys()` ，该方法和 `forEach` 结合使用之便利对象自己的属性。

```
function Foo() {
	this.name = 'foo',
	this.age = '18'
};

Foo.prototype.geAge = function() {
	console.log(this.name)
};

function Bar() {
	this.name = 'bar';
};

Bar.prototype = new Foo();
Bar.prototype.constructor = Bar; // 修正Bar.prototype.constructor 指向它本身

// 实例化Bar
let myBar = new Bar();

Object.keys(myBar).forEach(function(key) {
	console.log('key:' + key + ', value:' + myBar[key])
});
/*
key:name, value: bar
 */
```

#### for of

在ES6中，我们要实现对象属性的遍历我们可以使用 `for … of` ，但是我们这里面临一个问题就是一般对象无法使用它，而像数组，字符串，类数组(arguments, NodeList)，以及ES6中的Set等则可以。原因很简单，是因为一般对象不是可迭代的对象。由于历史原因Object对象并没有这样的设计，我们以前遍历对象的属性用 `for … in` ，如果要实现一般对象的迭代我们可以在 `Object.prototype` 上添加 `[Symbobl.iterator]()` 方法即可。

iterator的作用就是将迭代行为与集合(collection)本身分离。而 `[Symbobl.iterator]()` 刚好提供了默认的迭代器，那么如何实现这个默认的迭代行为呢？有的人会不加思索的说照搬 `for … in` 的即可，但是  `for … in` 遍历对象的属性有一个地方一直为人诟病的是，我们有很多场合要使用对象自己的属性，而非继承的属性，所以开发中我们不得不使用 `hasOwnProperty` 来进行过滤。而在后来JavaScript中引入 `Object.keys()` 方法来过滤属性。

总结起来我们实现一般对象要从三个方面入手：

* 是否包含原型链上的继承属性
* key(数组中的索引)是String 还是包括symbol
* 对象是否包含不可枚举的属性

这样的组合起来就有8种情况，具体使用我们可以根据自己的实际需要进行自定义。

ES6中的迭代器实现有点儿麻烦，使用迭代器就相当于重新定义新的数据类型，在ES6中新定义的Map，Set等都使用了迭代器，但是在开发中我们一般不这样做，我们可以将一般对象转化为含有迭代器的可迭代对象，比如我们常见处理方式将对象转化为数组。

```
let obj = { a:1, b:2, c:3 };

for (let key of Object.keys(obj)) {
	console.log(key, obj[key]);
}

/*
a 1
b 2
c 3
 */

```

### 使用注意问题

1.两个对象数据的比较

```
var obj = {
	a:1,
	b:2,
	c:3
}

var obj2 = {
	a:1,
	b:2,
	c:3
}

function compare(origin, target) {
    if (typeof target === 'object')    {
        if (typeof origin !== 'object') return false
        for (let key of Object.keys(target))
            if (!compare(origin[key], target[key])) return false
        return true
    } else {
    	return origin === target
    }
}
```

对于两个未知的对象我们使用递归来进行深度遍历比较是否存在不相等的属性，如果存在返回false；反之，则返回true。

我们再看看JavaScript开发中的其它常见数据类型的比较方式。

变量的比较我们通过 `===` 或 `!==` 来判断两个对象是否相等。

```
var a = 1;
var b = 1;
console.log(a === b); // true
```

两个数组中数字元素的比较

```
var a = [1, 2, 3, 4, 5];
var b = [1, 3, 5, 7, 9];
console.log(a[0] === b[0]); // true
console.log(a[1] === b[1]); // false
```

连个数组中的字符串元素比较

结果同上

两个数组中的对象元素的比较

```
var arr = [{a:1}, {b:2}, {c:3}, {d:4}, {e:5}];
var arr2 = [{a:1}, {c:3}, {e:5}];

console.log(arr[0] === arr2[0]); // false
console.log(arr[] === arr2[1]); // false
```


