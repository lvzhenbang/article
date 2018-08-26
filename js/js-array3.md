## 常用数组方法  (ES6及以后)

ES6 的方法及 ES6方法的 ES5 兼容

改变原数组：fill，
返回一个原数组副本的改变：

### ES6

新增的数组方法有的挂在Array对象上，有的挂在Array.prototype对象上。

#### Array.from(arrayLike, mapFn, thisArg)

描述：将一个类数组或可迭代对象转换为一个数组。参数arrayLike是要转换为数组的为数组或迭代对象，参数mapFn为数组中的每个元素执行回调函数，参数thisArg为执行mapFn时的对象。

兼容性实现：

```
if (!Array.from) {
  Array.from = (function () {
    var toStr = Object.prototype.toString;
    var isCallable = function (fn) {
      return typeof fn === 'function' || toStr.call(fn) === '[object Function]';
    };
    var toInteger = function (value) {
      var number = Number(value);
      if (isNaN(number)) { return 0; }
      if (number === 0 || !isFinite(number)) { return number; }
      return (number > 0 ? 1 : -1) * Math.floor(Math.abs(number));
    };
    var maxSafeInteger = Math.pow(2, 53) - 1;
    var toLength = function (value) {
      var len = toInteger(value);
      return Math.min(Math.max(len, 0), maxSafeInteger);
    };

    return function from(arrayLike, mapFn, thisArg) {
      var C = this;

      var items = Object(arrayLike);

      if (arrayLike == null) {
        throw new TypeError("Array.from requires an array-like object - not null or undefined");
      }

      var mapFn = arguments.length > 1 ? arguments[1] : void undefined;
      var T;
      if (typeof mapFn !== 'undefined') {
       if (!isCallable(mapFn)) {
          throw new TypeError('Array.from: when provided, the second argument must be a function');
        }

        if (arguments.length > 2) {
          T = arguments[2];
        }
      }

      var len = toLength(items.length);
      var A = isCallable(C) ? Object(new C(len)) : new Array(len);
      var k = 0;
      var kValue;
      while (k < len) {
        kValue = items[k];
        if (mapFn) {
          A[k] = typeof T === 'undefined' ? mapFn(kValue, k) : mapFn.call(T, kValue, k);
        } else {
          A[k] = kValue;
        }
        k += 1;
      }
      A.length = len;
      return A;
    };
  }());
}
```

用法：

```
let str = "hello";
Array.from(str); // ["h", "e", "l", "l", "o"]

var fn = function() {
	return Array.from(arguments)
}

fn(1, 2, 3) // [1, 2, 3]
```

#### Array.of(element, element2, element...)

描述：创建一个具有可变参数的数组。

兼容性实现：

```
if (!Array.of) {
  Array.of = function() {
    return Array.prototype.slice.call(arguments);
  };
}
```

用法：

```
Array.of(1, 2, 3) // [1, 2, 3]
```

### Array.isArray(obj)

描述：用于确定传入的值是否为一个数组。如果传入的对象是数组返回true，否则返回false。

兼容性实现：

```
if (!Array.isArray) {
  Array.isArray = function(arg) {
    return Object.prototype.toString.call(arg) === '[object Array]';
  };
}
```

用法：

```
var arr = [1, 2, 3]
Array.isArray(arr) // true

var obj = {}
Array.isArray(obj) // false
```


#### Array.prototype.fill(varlue, start, end)

描述：将一个数组中的所有元素都赋值为固定的一个值。参数value为填充数组的值，参数start为起始索引值（默认值为0），参数end为结束索引值（默认值为数组长度）。

注意：前闭后开

兼容性实现：

```
if (!Array.prototype.fill) {
	Array.prototype.fill = function(value) {
		if (this == null) {
			throw new TypeError('this is null or not defined');
		}

		var O = Object(this),
			len = O.length >>> 0,
			start = arguments[1];
			end = arguments[2];

		var first = relativeStart < 0 ?
			Math.max(len + relativeStart, 0) :
			Math.min(relativeStart, len);

		var relativeStart = start === undefined ? 0 : start >> 0;
		var relativeEnd = end === undefined ? len : end >> 0;

		var last = relativeEnd < 0 ?
			Math.max(len + relativeEnd, 0) :
			Math.min(relativeEnd, len);

		while (first < last) {
			O[first] = value;
			first++;
		}

		return O;
	});
}
```

用法：

```
var arr = [1, 2, 3, 4, 5, 6];

arr.fill('*', 1, 4) // [1, "*", "*", "*", 5, 6]
```

#### Array.prototype.includes  ES6

描述：它可以非常简单的查找到数组某一项元素是否在Array中，特别是对于NaN。在ES6前我们使用indexOf/lastIndexOf，而它们不能查找NaN。

用法：

```
const arr = [1, 2, 3, NaN]
arr.includes(NaN) // true

```

兼容性实现：

```
```

### 指数运算符 `**` ES6

描述：除了我们所熟知的 `+`, `-`, `*`, `/`, `%`这些中缀运算符外，`**` 这个中缀运算符通常用于指数运算

用法：

```
// 在ES6前我们用Math.pow()
Math.pow(2, 3) // 8

// 现在我们用 **
2**3 // 8
```

### Object.values() ES8

描述：它返回对象自身所有的属性值，而不包括原型链上的任何值。

用法：

```
var Robot = function(name) {
  this.name = name
}

Robot.prototype.say = function() {
  console.log('My name is ' + this.name + '.')
}

var robert = new Robot('robert')

// 在ES5中我们使用Object.keys() 来获取对象属性的属性名，但是它也不包含对象继承的原型链上的。
Object.keys(robert) // ["name"]

// 在ES7中我们使用Object.values() 来获取对象属性的属性值
Object.values(robert) // ["robert"]
```

### Object.entries() ES8

描述：它以数组的形式返回对象的键和值。

```
const person = { name: 'robert', age: 18 }
for (let [key, value] of Object.entries(person)) {
  console.log(`key: ${key}, value: ${value}`)
}
/**
 * key: name, value: robert
 * key: age, value: 18
 */
```