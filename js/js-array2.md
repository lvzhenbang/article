
### ES5

ES5 的方法及 ES5方法的 ES3 兼容

改变原数组：
返回一个原数组副本的改变：map，reduce（reduceRight），filter，

#### array.map(function(currentValue, index, arr)){}， thisArg)

描述：将数组中的每个元素传给指定的函数。返回一个数字组，它包含该函数的返回值。回调函数中的currentValue，为当前循环的元素值，index为当前循环中元素的索引，arr为当前的数组

注意：map不会对空数组进行检查，也不会改变原数组，thisValue指向使用该方法的对象，忽略为undefined

兼容性实现：

```
if (!Array.prototype.map) {
  Array.prototype.map = function(callback, thisArg) {

    var that = thisArg || null,
    	i = 0,
    	oldArr = Object(this),
    	len = O.length >>> 0,
    	newArr = new Array(len);

    if (this == null) {
      throw new TypeError(" this is null or not defined");
    }

    // 如果callback不是函数,则抛出TypeError异常.
    if (Object.prototype.toString.call(callback) != "[object Function]") {
      throw new TypeError(callback + " is not a function");
    }

    while(i < len) {
      var kValue,
      	  mValue;

      if (i in oldArr) {
        kValue = oldArr[i];
        mValue = callback.call(that, kValue, i, O);
        newArr[i] = mValue;
      }
      i++;
    }

    return newArr;
  };      
}
```

用法：

```
var arr = [1, 2, 3, 4, 5];

var db = function(arr) {
	return arr.map(function(v) {
		return v * 2
	})
}

db(arr); // [2, 4, 6, 8, 10]
arr // [1, 2, 3, 4, 5]
```

#### array.reduce(callback[, initalValue])

描述：该方法的第一个参数是一个回调函数，这个回调函数有4个参数，求和值、当前值，当前值索引，数组本身；第二个参数是个可选参数，表示初始值，若指定，则当作最初使用的求和值，若缺省，则使用数组第一个元素作为求和值，相比较于有第二个参数的情况少迭代一次。 

注意：改变原数组。

array.reduce 的兼容性实现：

```
if (typeof Array.prototype.reduce !== "function") {
  Array.prototype.reduce = function (fn, initialValue) {
     var acc = initialValue, k = 0, len = this.length;
     if (typeof initialValue === "undefined") {
        acc = this[0];
        k = 1;
     }
     
    if (typeof fn === "function") {
      for (k; k < len; k++) {
         this.hasOwnProperty(k) && (acc = fn(acc, this[k], k, this));
      }
    }
    return acc;
  };
}
```

用法：

二维数组的扁平化：

```
let martrix = [[1, 2], [3, 4], [5, 6]];

let flattern = arr => arr.reduce((acc, cur) => acc.concat(cur) , []);

flattern(martrix); // [1, 2, 3, 4, 5, 6]

```

二维数组的扁平化方法2：

```
let martrix = [[1, 2], [3, 4], [5, 6]];

let flattern = arr => [].concat.apply([], arr);

flattern(martrix); // [1, 2, 3, 4, 5, 6]

```

将url地址的参数像转化为对象，

```
const getURLParameters = url =>
  url.match(/([^?=&]+)(=([^&]*))/g).reduce(
    (a, v) => (a[v.slice(0, v.indexOf('='))] = v.slice(v.indexOf('=') + 1), a), {}
  );
getURLParameters('http://url.com/page?name=Adam&surname=Smith') // {name: 'Adam', surname: 'Smith'}
```


#### array.filter(callback[, thisObject])

描述：返回过滤后的新数组。第一个参数callback 回调函数有三个参数，当前值、当前值索引、该数组，它返回布尔值 true/false ，如果是true，则保留；如果是false，则删除；第二个参数可选，它作为回调函数的this使用，如果省略，回调函数的this为undefined。

注意：不改变原数组。

兼容性实现：

```
if (typeof Array.prototype.filter !== "function") {
  Array.prototype.filter = function (fn, context) {
    var arr = [];
    if (typeof fn === "function") {
       for (var k = 0, len = this.length; k < len; k++) {
          fn.call(context, this[k], k, this) && arr.push(this[k]);
       }
    }
    return arr;
  };
}
```

用法：

```
let arr = [1, 2, 3, 4, 5, 6];
let arrayFilter = arr => arr.filter(value => value%2 === 0);
arrayFilter(arr); // [2, 4, 6]
```

#### Array.prototype.every(function(value, index, arr){}, thisArg)

描述：测试数组中的所有元素是否都通过了指定函数的测试，如果有一个元素未通过测试就返回false，否则返回true。参数function()用来测试每个元素的函数(其中value为元素的值，index为元素索引，arr为原数组)，参数thisArg改变函数的this指向。

兼容性实现：

// void 0 === undefined，在一些undefined中不使用undefined是因为在ES5之前window下的undefined是可以被修改的，所以某些特殊情况下会造成使用undefined的错误，所以void 0 是为了防止undefined被重写后，出现判断不准确的情况。

```
if (!Array.prototype.every){
  Array.prototype.every = function(fun , thisArg){
    if (this === void 0 || this === null)
      throw new TypeError();

    var t = Object(this),
    	len = t.length >>> 0;
    if (typeof fun !== 'function')
        throw new TypeError();

    var thisArg = arguments.length >= 2 ? arguments[1] : void 0;
    for (var i = 0; i < len; i++) {
      if (i in t && !fun.call(thisArg, t[i], i, t))
        return false;
    }

    return true;
  };
}
```

用法：

```
function isGreater5 (value) {
	return value > 5
}

var arr = [3, 5, 7, 9];

arr.every(isGreater5) // false
```

### Array.prototype.some(function(value, index, arr), thisArg)

它与Array.prototype.every()相反只要有一个元素满足条件就返回true，否则返回false。

### Array.prototype.indexOf(searchElement, fromIndex)

描述：返回数组中可以查找到的第一个给定元素的索引值。参数searchElement为查找的元素，参数fromIndex为查找的起始位置，忽略该参数则为0，如果fromIndex值大于数组长度，直接返回-1。如果查找到给定的元素，返回首个查找到的元素的索引值，否则返回-1.

兼容性实现：

```
if (!Array.prototype.indexOf) {
  Array.prototype.indexOf = function(searchElement, fromIndex) {

    if (this == null) {
      throw new TypeError('"this" is null or not defined');
    }

    var k,
    	O = Object(this),
    	len = O.length >>> 0,
    	n = +fromIndex || 0;

    if (len === 0) {
      return -1;
    }

    if (Math.abs(n) === Infinity) {
      n = 0;
    }

    if (n >= len) {
      return -1;
    }
    k = Math.max(n >= 0 ? n : len - Math.abs(n), 0);

    while (k < len) {
      if (k in O && O[k] === searchElement) {
        return k;
      }
      k++;
    }
    return -1;
  };
}
```

用法：

```
arr = [1, 2, 3, 4, 5, 6];

arr.indexOf(2); // 1
arr.indexOf(2, -5); // 1
arr.indexOf(7); // -1
```

### Array.prototype.lastIndexOf(searchElement, fromIndex)

与Array.prototype.indexOf(searchElement, fromIndex)查找方向相反，为逆向查找。

### Array.prototype.forEach(function(value, index, arr){}, thisArg)

描述：按升序为数组中含有效值的每一项执行一次callback函数，那些已删除（使用delete方法等情况）或者未初始化的项将被跳过（但不包括那些值为 undefined 的项）。或调函数有三个参数value为遍历的当前元素值，index为当前元素的索引值，arr为原数组，thisArg为回调函数的使用对象指向。

兼容性实现：

```
if (!Array.prototype.forEach) {
  Array.prototype.forEach = function(callback, thisArg) {
    if (this == null) {
      throw new TypeError(' this is null or not defined');
    }
    var T,
      k = 0,
      O = Object(this),
      len = O.length >>> 0;

    if (typeof callback !== "function") {
      throw new TypeError(callback + ' is not a function');
    }

    if (arguments.length > 1) {
      T = thisArg;
    }

    while (k < len) {
      var kValue;
      if (k in O) {
        kValue = O[k];
        callback.call(T, kValue, k, O);
      }

      k++;
    }
  };
}
```

用法：

```
var arr = [1, 2, 3];

arr.forEach(function(v, i) {
  console.log('index: ' + i + ', value: ' + v)
})

/**
 * index: 0, value: 1
 * index: 1, value: 2
 * index: 2, value: 3
 */
```


