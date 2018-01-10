## 常用数组方法

改变原数组：sort
返回一个原数组副本的改变：slice

### array.sort(sortby)

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

### array.slice(start, end)

描述：返回指定范围的数组。第一个参数必选（可为负数），第二个参数可选（若指定值，就是到指定值得范围，否则就是数组长度减1位置）。范围特性是前闭后开。

注意：返回数组副本的改变。

用法：

```
var arr = [1, 2, 3, 4, 5, 6];

arr.slice(1, 3) // [2, 3]

arr.slice(1, -3) // [2, 3]
```

### array.splice(index, num, ...item)

描述：从数组中删除指定数量的元素，向数组某一个或任意个元素。参数 `index` （它既是删除元素的起始位置，也是添加元素的位置）和 `num` （删除元素的个数）不可省略，第三个参数可省略。 

注意：改变原数组。

用法：

```
let arr = [1, 2, 3, 4, 5, 6];

arr.splice(1, 3); // [1, 5, 6]

arr.splice(arr.length, 0, 7, 8, 9); // [1, 5, 6, 7, 8, 9]
```
