## 常用数组方法  (ES6前)

ES5 的方法及 ES5方法的 ES3 兼容

改变原数组：sort，splice，ruduce
返回一个原数组副本的改变：slice，filter

### ES3

#### array.sort(sortby)

描述：数组排序，但是我们有时候使用数组的参数sortby进行一些复杂的数组处理。

注意：该方法是对数组的引用，在原数组上进行排序，不是使用数组的副本。

用法：

```
var arr = [1, 3, 2, 5, 4];

arr.sort(); // [1, 2, 3, 4, 5]
```

```
let arr = [
	{name: "li", age: 10 },
	{name: "wang", age: 12 },
	{name: "zhang", age: 11 },
	{name: "zhao", age: 10 },
	{name: "qian", age: 9 }	
];

let sortBy = (a, b) => a.age - b.age;

arr.sort(sortBy);
// [{name: "qian", age: 9}, {name: "li", age: 10}, {name: "zhang", age: 11}, {name: "zhang", age: 11}, {name: "wang", age: 12}]
```

#### array.slice(start, end)

描述：返回指定范围的数组。第一个参数必选（可为负数），第二个参数可选（若指定值，就是到指定值得范围，否则就是数组长度减1位置）。范围特性是前闭后开。

注意：返回数组副本的改变。

用法：

```
var arr = [1, 2, 3, 4, 5, 6];

arr.slice(1, 3) // [2, 3]

arr.slice(1, -3) // [2, 3]
```

#### array.splice(index, num, ...item)

描述：从数组中删除指定数量的元素，向数组某一个或任意个元素。参数 `index` （它既是删除元素的起始位置，也是添加元素的位置）和 `num` （删除元素的个数）不可省略，第三个参数可省略。 

注意：改变原数组。

用法：

```
let arr = [1, 2, 3, 4, 5, 6];

arr.splice(1, 3); // [1, 5, 6]

arr.splice(arr.length, 0, 7, 8, 9); // [1, 5, 6, 7, 8, 9]
```

### ES5

#### array.reduce(callback[, initalValue]) // v1.8

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

将url地址的参数像转化为对象，

```
const getURLParameters = url =>
  url.match(/([^?=&]+)(=([^&]*))/g).reduce(
    (a, v) => (a[v.slice(0, v.indexOf('='))] = v.slice(v.indexOf('=') + 1), a), {}
  );
getURLParameters('http://url.com/page?name=Adam&surname=Smith') // {name: 'Adam', surname: 'Smith'}
```


#### array.filter(callback[, thisObject]) // v1.6

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