# JavaScript 自己提供的乐行判断

## typeof

如果不对对象做严格区分使用type。

number: 

	typeof 1; // "number"

string:

	typeof 'hello world'; // "string"

boolean:

	typeof true; // "boolean"

undefined：

	typeof undefined; // "undefined"

null:

	typeof null; // "undefined" (特别)

object:
	
	typeof {};  // "object"

Symbol：
	
	typeof Symbol(); // "symbol"

null要单独处理，就像jQuery中一样，如果要判断的对象为null，就直接返回 `String(obj)` 。

因此，可以写一个判断基本类型的方法：

```
var type = function(obj) {
	if(obj === null) return String(obj);
	return typeof obj;
}
```

## instanceof

instanceof 是用来明确对象为某种特定类型的方法。

instanceof 的实现使用了原型继承的L表示左表达式，R表示右表达式，它是用L.__proto__ 是否等于 R.prototype 来判断对象的类型的。

String:

```
var str = new String('hello world');

str instanceof String // true
```

String:

```
var str = new Number('10');

str instanceof String // true
```

Datte:

```
var o = new Date();

o instanceof Date; // true
```

Array:

```
var o = new Array();

o instanceof Array; // true
```

RegExp:

```
var reg = new RegExp('/^[a-z]$/');

reg instanceof RegExp; // true
```

Object:

```
var o = new Object();

o instanceof Object; // true

var o2 = {};

o2 instanceof Object; // true
```

Function:

```
var o = function() {};

o instanceof Function; // true

```

Func(自定义):

```
function Func() {}

var o = new Func();

o instanceof Func; // true
```

instanceof 即可以验证自定义对象，也可以验证内置对象类型，但有一点我们要注意，那就是他们本身就是对象。

```
var str = new String('hello world');

str instanceof Object; // true
```

我们在一开始就说明了instanceof的实现原理，知道instanceof左边是右边的实例，所以我们可以用如下代码获取对象的类型名：

	obje.__proto__.constructor.name;

因此，我们可以写一个判断对象类型的方法如下：

```
objectType = function(obj) {
	if(typeof obj === 'object' || typeof obj === 'function') {
		return obj.__proto__.constructor.name;
	}
}
```

上面的方法虽好，但是ie浏览器不支持 `__proto__`（这个属性也是我们判断浏览器与非浏览器的常用方式）。

下面介绍一个万能的方法。

## Object.prototype.toString.call(obj)

```
Object.prototype.toString.call('hello world'); // "[object String]"

Object.prototype.toString.call(1); // "[object Number]"

Object.prototype.toString.call(true); // "[object Boolean]"

Object.prototype.toString.call(null); // "[object Null]"

Object.prototype.toString.call(); // "[object Undefined]"

Object.prototype.toString.call([]); // "[object Array]"

Object.prototype.toString.call({}); // "[object Object]"

Object.prototype.toString.call(new Date()); // "[object Date]"

Object.prototype.toString.call(/test/i); // "[object RegExpArray]"

Object.prototype.toString.call(function () {}); // "[object Function]"
```

获取数据类型的代码如下：

```
var core_toString = Object.prototype.toString;
var getObjectType = function(obj) () {
	return core_toString.call(obj).slice(8, -1);
}
```

这个方法可以判断所有的数据类型，也是官方推荐的，但是在实际的开发中，我们使用 `typeof` 来判断基本类型，用 `Objet.prototype.toString.call(obj)` 判断引用类型。

## 常见框架和库的实数据类型判断

jQuery：

```
var class2type = {};
var core_toString = Object.prototype.toString;

"Boolean Number String Function Array Date RegExp Object Error".split(' ').forEach(function(name, i) {
	class2type["[object " + name + "]"] = name.toLowerCase();
});

var getType = function (obj) {
	if (obj == null) {
		return String(obj);
	}
	return typeof obj === "object" || typeof obj === "function" ?
		class2type[ core_toString.call(obj) ] || "object" :
		typeof obj;
};
// 测试
getType(function(){}); // "function"
getType([]); // "array"
```

这里将jQuery的实现原理抽取出来，用原生js实现。

vue.js

```

/**
 * 判断是否为基本数据类型
 */
function isPrimitive (value) {
  return (
    typeof value === 'string' ||
    typeof value === 'number' ||
    // $flow-disable-line
    typeof value === 'symbol' ||
    typeof value === 'boolean'
  )
}

/**
 * 判断是否为普通对象
 */
function isObject (obj) {
  return obj !== null && typeof obj === 'object'
}

/**
 * 获取原生类型，如： [object Object]
 */
var _toString = Object.prototype.toString;

function toRawType (value) {
  return _toString.call(value).slice(8, -1)
}

/**
 * 普通对象
 */
function isPlainObject (obj) {
  return _toString.call(obj) === '[object Object]'
}
/**
 * 正则对象
 */
function isRegExp (v) {
  return _toString.call(v) === '[object RegExp]'
}

```

angular2：

```
function isPresent(obj) {
return obj !== undefined && obj !== null;
}
exports.isPresent = isPresent;
function isBlank(obj) {
return obj === undefined || obj === null;
}
exports.isBlank = isBlank;
function isBoolean(obj) {
return typeof obj === "boolean";
}
exports.isBoolean = isBoolean;
function isNumber(obj) {
return typeof obj === "number";
}
exports.isNumber = isNumber;
function isString(obj) {
return typeof obj === "string";
}
exports.isString = isString;
function isFunction(obj) {
return typeof obj === "function";
}
exports.isFunction = isFunction;
function isType(obj) {
return isFunction(obj);
}
exports.isType = isType;
function isStringMap(obj) {
return typeof obj === 'object' && obj !== null;
}
exports.isStringMap = isStringMap;
function isPromise(obj) {
return obj instanceof _global.Promise;
}
exports.isPromise = isPromise;
function isArray(obj) {
return Array.isArray(obj);
}
exports.isArray = isArray;
function isDate(obj) {
return obj instanceof exports.Date && !isNaN(obj.valueOf());
}
exports.isDate = isDate;
```

我们常见的就是这几种实现方式，或是这几种方式的混合（zepto.js）。
