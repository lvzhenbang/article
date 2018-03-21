## 常用数组方法 


改变原数组：pop，push，unshift，shift，sort，splice，reverse
返回一个原数组副本的改变：slice，join，contact，toString，toLocaleString

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


#### array.join(seperator)

描述：该方法用于把数组中的所有元素放入一个字符串，用一个可选的分隔符参数进行连接。

注意：不改变原数组。

兼容性实现：

```
if (typeof Array.prototype.join !== "function") {
  Array.prototype.join = function (c) {
    var str = '',
        len = this.length;
    if(len <= 1) return this[0]

    if(c) {
      str = this[0]
      for (var i = 1; i < len; i++) {
          str = str + c + this[i]
      }
    }
    return str;
  }
}
```

用法：

```
var arr = [1, 2, 3, 4, 5, 6];
arr.join('-') // "1-2-3-4-5-6"
```

#### array.reverse()

描述：用于颠倒数组中元素的顺序。

注意：不改变原数组。

兼容性实现：

```
if (typeof Array.prototype.reverse !== "function") {
  Array.prototype.reverse = function () {
    var arr = [],
        len = this.length;

    for(var i=len-1; i>=0; i--) {
      arr[len-i] = this[i]
    }
    return arr;
  }
}
```

用法：

```
var arr = [1, 2, 3, 4, 5, 6];
arr.reverse() // [6, 5, 4, 3, 2, 1]
```

#### array.concat()

描述：将两个或多个数组拼接成一个数组。参数可以是数组也可以是非数组元素。如果参数是非嵌套的数组，
可以将数组元素进行拼接；如果参数是嵌套数组，那么该方法不会扁平化该参数数组。

注意：不改变原数组。

兼容性实现：

```
if (typeof Array.prototype.concat !== "function") {
  Array.prototype.concat = function () {
    var arr = []
    for(var i=0, i=arguments.length; i<len; i++) {
      arr[i] = arguments[0]
    }
    return arr;
  }
}
```

用法：

```
var arr = [1, 2, 3];
arr.concat([4, 5, 6]) // [1, 2, 3, 4, 5, 6]
```

#### array.push() && array.pop()

描述：push()向数组的末尾添加一个或多个元素，返回数组为数组长度；pop()用于删除并返回数组的最后一个元素，返回值为当前数组最后一个元素。

说明：如果数组为空，pop()返回undefined

用法：

```
var arr = [1, 2, 3];
arr.push(4, 5, 6) // 6， arr => [1, 2, 3, 4, 5, 6]
arr.pop() // 6， arr => [1, 2, 3, 4, 5]
```

#### array.unshift() && shift()

描述：unshift()向数组的头部添加一个或多个元素，返回数组为数组长度；shift()用于删除并返回数组的最后一个元素，返回值为当前数组最后一个元素。

说明：如果数组为空，shift()返回undefined

用法：

```
var arr = [4, 5, 6];
arr.unshift(1, 2, 3) // 6， arr => [1, 2, 3, 4, 5, 6]
arr.shift() // 1， arr => [2, 3, 4, 5, 6]
```

#### toString()

描述：可以将一个逻辑转化为字符串。返回值会根据原始的布尔值或者booleanObject对象返回相应的字符串“true”或“false”

说明：如果该方法的对象不是Boolean，则抛出异常TypeError

用法：

```
var boo = new Boolean(true)
boo.toString() // "true"

var arr = [1, 2, 3]
arr.toString() // "1,2,3"

var obj = {}
obj.toString() // "[object Object]"
```

#### toLocaleString()

描述：可根据本地时间把Date对象转换为字符串，并返回结果

说明：dateObject 的字符串表示，以本地时间区表示，并根据本地规则格式化。

用法：

```
var date = new Date()
date.toLocaleString() // "2018/3/13 下午3:02:31"
```