
## Array

### arrayGcd

Calculates the greatest common denominator (gcd) of an array of numbers.

Use `Array.reduce()` and the `gcd` formula (uses recursion) to calculate the greatest common denominator of an array of numbers.

```js
const arrayGcd = arr =>{
  const gcd = (x, y) => !y ? x : gcd(y, x % y);
  return arr.reduce((a,b) => gcd(a,b));
}
// arrayGcd([1,2,3,4,5]) -> 1
// arrayGcd([4,8,12]) -> 4
```

### arrayLcm

Calculates the lowest common multiple (lcm) of an array of numbers.

Use `Array.reduce()` and the `lcm` formula (uses recursion) to calculate the lowest common multiple of an array of numbers.

```js
const arrayLcm = arr =>{
  const gcd = (x, y) => !y ? x : gcd(y, x % y);
  const lcm = (x, y) => (x*y)/gcd(x, y); 
  return arr.reduce((a,b) => lcm(a,b));
}
// arrayLcm([1,2,3,4,5]) -> 60
// arrayLcm([4,8,12]) -> 24
```

### arrayMax

返回一个数组中的最大值。

使用 `Math.max()` 结合spread操作符 (`...`) 去获取数组中的最大值.

```js
const arrayMax = arr => Math.max(...arr);
// arrayMax([10, 1, 5]) -> 10
```

### arrayMin

返回数组中的最小值.

用 `Math.min()` 集合spread操作符 (`...`) 去获取数组中的最小值.

```js
const arrayMin = arr => Math.min(...arr);
// arrayMin([10, 1, 5]) -> 1
```

### chunk

将一个数组分成指定长度的块数组.

用 `Array.from()` 生成一个适当长度的块数组,然后遍历整个块数组.
用 `Array.slice()` 分割原数组向块数组中的块中插入元素，块的长度为 `size`.
如果原数组最终不能在进行分割，那么这个块将包含剩余的所有元素.

```js
const chunk = (arr, size) =>
  Array.from({length: Math.ceil(arr.length / size)}, (v, i) => arr.slice(i * size, i * size + size));
// chunk([1,2,3,4,5], 2) -> [[1,2],[3,4],[5]]
```

### compact

将数组中的非真值移除.

用 `Array.filter()` 去移除非真值 (`false`, `null`, `0`, `""`, `undefined`, and `NaN`).

```js
const compact = arr => arr.filter(Boolean);
// compact([0, 1, false, 2, '', 3, 'a', 'e'*23, NaN, 's', 34]) -> [ 1, 2, 3, 'a', 's', 34 ]
```

注：Boolean的初始值为false

### countOccurrences

计算一个值在数组中出现的次数.

用 `Array.reduce()` 去计算每次遇到的特定值的次数.

```js
const countOccurrences = (arr, value) => arr.reduce((a, v) => v === value ? a + 1 : a + 0, 0);
// countOccurrences([1,1,2,1,2,3], 1) -> 3
```


### deepFlatten

抹平数组(没有嵌套的数组).

使用递归.
用 `Array.concat()` 拼接一个空数组 (`[]`) 然后使用spread操作符 (`...`) 去抹平数组.

```js
const deepFlatten = arr => [].concat(...arr.map(v => Array.isArray(v) ? deepFlatten(v) : v));
// deepFlatten([1,[2],[[3],4],5]) -> [1,2,3,4,5]
```

### difference

返回的数组中元素来自于源数组，但不包含目标数组中的元素.

将目标数组转化为一个 `Set`, 然后用 `Array.filter()` 过滤原数组返回不在目标数组中的元素.

```js
const difference = (a, b) => { const s = new Set(b); return a.filter(x => !s.has(x)); };
// difference([1,2,3], [1,2,4]) -> [3]
```

```我的修改1
const difference2 = (a, b) => a.filter(x => b.indexOf(x) === -1)
// difference2([1,2,3], [1,2,4]) -> [3]
```

```我的修改2
const difference2 = (a, b) => a.filter(x => !b.includes(x))
// difference2([1,2,3], [1,2,4]) -> [3]
```

### differenceWith

返回的数组中元素来自于源数组和目标数组中的元素经过`comp`后的元素.

用 `Array.filter()` 和 `Array.find()` 去找出合适的元素.

```js
const differenceWith = (res, dest, comp) => res.filter(a => !dest.find(b => comp(a, b)))
// differenceWith([1, 1.2, 1.5, 3], [1.9, 3], (a,b) => Math.round(a) == Math.round(b)) -> [1, 1.2]
```

### distinctValuesOfArray

数组去重.

用 ES6 `Set` 和 `...rest` 操作符去除所有重复的元素.

```js
const distinctValuesOfArray = arr => [...new Set(arr)];
// distinctValuesOfArray([1,2,2,3,4,4,5]) -> [1,2,3,4,5]
```

### dropElements

移除数组中的元素直到`func`返回 `true`，返回使数组中剩余的元素.

循环数组, 判断`func`是否返回 `true`，如果否使用 `Array.slice()` 移除数组中的第一个元素，直到 `function` 返回 `true`; 否则，直接返回数组.

```js
const dropElements = (arr, func) => {
  while (arr.length > 0 && !func(arr[0])) arr.splice(0,1);
  return arr;
};
// dropElements([1, 2, 3, 4], n => n >= 3) -> [3,4]
```

```我的修改
const dropElements = (arr, func) => {
  while (arr.length > 0 && !func(arr[0])) arr.splice(0, 1);
  return arr;
};
dropElements([4, 3, 2, 1], n => n >= 3)
```

移除数组中的元素如果`func`返回 `true`

使用 `Array.filter()`来进行数组过滤

```我的扩展
const dropElements = (arr, func) => arr.filter(!func)
dropElements([4, 3, 2, 1], n => n >= 3)
```


### dropRight

返回一个从用变去除 `n` 个元素的数组

如果 `n` 小于数组的长度用 `Array.slice()` 去分割数组，然后返回分割得到的数组，否则返回一个空数组.

```js
const dropRight = (arr, n = 1) => n < arr.length ? arr.slice(0, arr.length - n) : []
// dropRight([1,2,3]) -> [1,2]
// dropRight([1,2,3], 2) -> [1]
// dropRight([1,2,3], 42) -> []
```

``` 我的修改（不建议使用，虽然代码的两区别不大，但是经过测试没有上一个快）
const dropRight = (arr, n = 1) => n < arr.length ? arr.splice(0, arr.length - n) : []
// dropRight([1,2,3])
```

### everyNth

每遍历 `n` 个元素，返回一个元素.

用 `Array.filter()` 去筛选出一个数组，筛选条件是每遍历 `n` 个元素，返回一个元素.

```js
const everyNth = (arr, nth) => arr.filter((e, i) => i % nth === nth - 1);
// everyNth([1,2,3,4,5,6], 2) -> [ 2, 4, 6 ]
```

### filterNonUnique

Filters out the non-unique values in an array.

Use `Array.filter()` for an array containing only the unique values.

```js
const filterNonUnique = arr => arr.filter(e => arr.indexOf(e) !== -1);
// filterNonUnique([1,2,2,3,4,4,5]) -> [1,3,5]
```

### flatten

抹平数组.

用一个空数组和spread `...` 操作符来生成一个没有嵌套的数组.

```js
const flatten = arr => [].concat( ...arr );
// flatten([1,[2],3,4]) -> [1,2,3,4]
```

### flattenDepth

抹平数组取决于指定的值 `depth`.

通过递减 `depth`，然后使用递归来完成.
用 `Array.reduce()` 和 `Array.concat()` 来合并元素或者数组.
默认 `depth` 值为 `1` 时停止递归.
省略定义 `depth`，将返回数组本身.

```js
const flattenDepth = (arr, depth = 1) =>
  depth != 1 ? arr.reduce((a, v) => a.concat(Array.isArray(v) ? flattenDepth(v, depth - 1) : v), [])
  : arr.reduce((a, v) => a.concat(v), []);
// flattenDepth([1,[2],[[[3],4],5]], 2) -> [1,2,[3],4,5]
```

### groupBy

根据给定的函数来对数组中的元素进行分组.

用 `Array.map()` 依据 `func` 来遍历数组中的元素.
用 `Array.reduce()` 创建一个对象, 对象中的key是map的结果.

```js
const groupBy = (arr, func) =>
  arr.map(typeof func === 'function' ? func : val => val[func])
    .reduce((acc, val, i) => { acc[val] = (acc[val] || []).concat(arr[i]); return acc; }, {});
// groupBy([6.1, 4.2, 6.3], Math.floor) -> {4: [4.2], 6: [6.1, 6.3]}
// groupBy(['one', 'two', 'three'], 'length') -> {3: ['one', 'two'], 5: ['three']}
```


### head

返回数组中的第一个元素.

用 `arr[0]` 返回传入数组的第一个元素.

```js
const head = arr => arr[0];
// head([1,2,3]) -> 1
```


### initial

返回数组中出最后一个外的所有元素.

用 `arr.slice(0,-1)` 来实现.

```js
const initial = arr => arr.slice(0, -1);
// initial([1,2,3]) -> [1,2]
```


### initialize2DArray

通过传入宽高和默认值来初始化一个二维数组.

用 `Array.map()` 去生成一个 `h` 列， 每列是一个长度为 `w`，默认值是 `value`的数组的二维数组. 如果默认值没有提供，那么默认值就为 `null`.

```js
const initialize2DArray = (w, h, val = null) => Array(h).fill().map(() => Array(w).fill(val));
// initializeArrayWithRange(2, 2, 0) -> [[0,0], [0,0]]
```

### initializeArrayWithRange

初始化一个数组，这个数组包含限定范围的数字 `start` 和 `end`.

用 `Array.from((end + 1) - start)` 去创建一个长度为`end - start + 1`的数组，然后用`Array.map()` 为初始化的数组赋值.
你可以省略 `start`，它的默认值是 `0`.

```js
const initializeArrayWithRange = (end, start = 0) => 
  Array.from({ length: (end + 1) - start }).map((v, i) => i + start);
// initializeArrayWithRange(5) -> [0,1,2,3,4,5]
// initializeArrayWithRange(7, 3) -> [3,4,5,6,7]
```

### initializeArrayWithValues

初始化一个长度为 `n` 数组,默认值为 `value` 的数组.

用 `Array(n)` 去创建一个长度为n，元素值为空的数组,然后用 `fill(v)` 为每个元素赋值为 `value`.
如果省略 `value` ,元素的默认值为 `0`.

```js
const initializeArrayWithValues = (n, value = 0) => Array(n).fill(value);
// initializeArrayWithValues(5, 2) -> [2,2,2,2,2]
```

### intersection

返回两个数组的共有元素.

将 `b` 转换为一个集合 `Set` , 然后用 `Array.filter()` 过滤掉 `a` 中包含 `b` 中的元素.

```js
const intersection = (a, b) => { const s = new Set(b); return a.filter(x => s.has(x)); };
// intersection([1,2,3], [4,3,2]) -> [2,3]
```

### last

返回数组中的最后一个元素.

用 `arr.length - 1` 作为给定数组最后一个元素的索引，返回该索引位置的元素.

```js
const last = arr => arr[arr.length - 1];
// last([1,2,3]) -> 3
```

### mapObject

用一个函数作为映射规则，将数组转化为一个对象, 对象的键值对由原始的的的值作为键，映射得到的值作为值.

使用内部匿名函数的作用域去声明一个undefined的内存空间, 用闭包去存储返回值. 用一个新 `Array` 存储原数组和经过 `fn` 映射的结果，然后用 `,` 操作符实现下一步的return操作, 无需上下文的切换 (由于闭包和操作顺序).

```js
const mapObject = (arr, fn) => 
  (a => (a = [arr, arr.map(fn)], a[0].reduce( (acc,val,i) => (acc[val] = a[1][i], acc), {}) )) ();
/*
const squareIt = arr => mapObject(arr, a => a*a)
squareIt([1,2,3]) // { 1: 1, 2: 4, 3: 9 }
*/
```

### nthElement

返回数组中的第 `n` 个元素.

用 `Array.slice()` 去获得包含第 `n` 个元素的数组，数组长度为1.
如果索引超出数组范围，返回一个 `[]`.
如果省略参数 `n`, 默认的获取数组的第一个元素.

```js
const nthElement = (arr, n = 0) => (n > 0 ? arr.slice(n,n+1) : arr.slice(n))[0];
// nthElement(['a','b','c'],1) -> 'b'
// nthElement(['a','b','b'],-3) -> 'a'
```

### pick

从对象中筛选出`arr`中指定的键值对.

用 `Array.reduce()` 去过滤并挑选出对象中所包含的键的键值对.

```js
const pick = (obj, arr) =>
  arr.reduce((acc, cur) => (cur in obj && (acc[cur] = obj[cur]), acc), {});
// pick({ 'a': 1, 'b': '2', 'c': 3 }, ['a', 'c']) -> { 'a': 1, 'c': 3 }
```

### pull

去除数组中的指定值.

用 `Array.filter()` 和 `Array.includes()` 清除数组中不需要的值.
用 `Array.length = 0` 初始化数组，然后用arr.push()放入过滤后剩余的数组元素 `pulled`.


```js
const pull = (arr, ...args) => {
  let argState = Array.isArray(args[0]) ? args[0] : args;
  let pulled = arr.filter((v, i) => !argState.includes(v));
  arr.length = 0; 
  pulled.forEach(v => arr.push(v));
};

// let myArray1 = ['a', 'b', 'c', 'a', 'b', 'c'];
// pull(myArray1, 'a', 'c');
// console.log(myArray1) -> [ 'b', 'b' ]

// let myArray2 = ['a', 'b', 'c', 'a', 'b', 'c'];
// pull(myArray2, ['a', 'c']);
// console.log(myArray2) -> [ 'b', 'b' ]
```

### pullAtIndex

Mutates the original array to filter out the values at the specified indexes.

Use `Array.filter()` and `Array.includes()` to pull out the values that are not needed.
Use `Array.length = 0` to mutate the passed in an array by resetting it's length to zero and `Array.push()` to re-populate it with only the pulled values.
Use `Array.push()` to keep track of pulled values 

```js
const pullAtIndex = (arr, pullArr) => {
  let removed = [];
  let pulled = arr.map((v, i) => pullArr.includes(i) ? removed.push(v) : v)
                  .filter((v, i) => !pullArr.includes(i))
  arr.length = 0; 
  pulled.forEach(v => arr.push(v));
  return removed;
}

// let myArray = ['a', 'b', 'c', 'd'];
// let pulled = pullAtIndex(myArray, [1, 3]);

// console.log(myArray); -> [ 'a', 'c' ]
// console.log(pulled); -> [ 'b', 'd' ]
```

### pullAtValue

Mutates the original array to filter out the values specified. Returns the removed elements.

Use `Array.filter()` and `Array.includes()` to pull out the values that are not needed.
Use `Array.length = 0` to mutate the passed in an array by resetting it's length to zero and `Array.push()` to re-populate it with only the pulled values.
Use `Array.push()` to keep track of pulled values 

```js
const pullAtValue = (arr, pullArr) => {
  let removed = [], 
    pushToRemove = arr.forEach((v, i) => pullArr.includes(v) ? removed.push(v) : v),
    mutateTo = arr.filter((v, i) => !pullArr.includes(v));
  arr.length = 0;
  mutateTo.forEach(v => arr.push(v));
  return removed;
}
/*
let myArray = ['a', 'b', 'c', 'd'];
let pulled = pullAtValue(myArray, ['b', 'd']);
console.log(myArray); -> [ 'a', 'c' ]
console.log(pulled); -> [ 'b', 'd' ]
*/
```


### remove

Removes elements from an array for which the given function returns `false`.

Use `Array.filter()` to find array elements that return truthy values and `Array.reduce()` to remove elements using `Array.splice()`.
The `func` is invoked with three arguments (`value, index, array`).

```js
const remove = (arr, func) =>
  Array.isArray(arr) ? arr.filter(func).reduce((acc, val) => {
    arr.splice(arr.indexOf(val), 1); return acc.concat(val);
    }, [])
  : [];
// remove([1, 2, 3, 4], n => n % 2 == 0) -> [2, 4]
```


### sample

Returns a random element from an array.

Use `Math.random()` to generate a random number, multiply it by `length` and round it of to the nearest whole number using `Math.floor()`.
This method also works with strings.

```js
const sample = arr => arr[Math.floor(Math.random() * arr.length)];
// sample([3, 7, 9, 11]) -> 9
```


### shuffle

Randomizes the order of the values of an array.

Use `Array.sort()` to reorder elements, using `Math.random()` in the comparator.

```js
const shuffle = arr => arr.sort(() => Math.random() - 0.5);
// shuffle([1,2,3]) -> [2,3,1]
```


### similarity

Returns an array of elements that appear in both arrays.

Use `filter()` to remove values that are not part of `values`, determined using `includes()`.

```js
const similarity = (arr, values) => arr.filter(v => values.includes(v));
// similarity([1,2,3], [1,2,4]) -> [1,2]
```


### symmetricDifference

Returns the symmetric difference between two arrays.

Create a `Set` from each array, then use `Array.filter()` on each of them to only keep values not contained in the other.

```js
const symmetricDifference = (a, b) => {
  const sA = new Set(a), sB = new Set(b);
  return [...a.filter(x => !sB.has(x)), ...b.filter(x => !sA.has(x))];
}
// symmetricDifference([1,2,3], [1,2,4]) -> [3,4]
```


### tail

Returns all elements in an array except for the first one.

Return `arr.slice(1)` if the array's `length` is more than `1`, otherwise, return the whole array.

```js
const tail = arr => arr.length > 1 ? arr.slice(1) : arr;
// tail([1,2,3]) -> [2,3]
// tail([1]) -> [1]
```


### take

Returns an array with n elements removed from the beginning.

Use `Array.slice()` to create a slice of the array with `n` elements taken from the beginning.

```js
const take = (arr, n = 1) => arr.slice(0, n);
// take([1, 2, 3], 5) -> [1, 2, 3]
// take([1, 2, 3], 0) -> []
```


### takeRight

Returns an array with n elements removed from the end.

Use `Array.slice()` to create a slice of the array with `n` elements taken from the end.

```js
const takeRight = (arr, n = 1) => arr.slice(arr.length - n, arr.length);
// takeRight([1, 2, 3], 2) -> [ 2, 3 ]
// takeRight([1, 2, 3]) -> [3]
```


### union

Returns every element that exists in any of the two arrays once.

Create a `Set` with all values of `a` and `b` and convert to an array.

```js
const union = (a, b) => Array.from(new Set([...a, ...b]));
// union([1,2,3], [4,3,2]) -> [1,2,3,4]
```


### without

Filters out the elements of an array, that have one of the specified values.

Use `Array.filter()` to create an array excluding(using `!Array.includes()`) all given values.

_(For a snippet that mutates the original array see [`pull`](#pull))_

```js
const without = (arr, ...args) => arr.filter(v => !args.includes(v));
// without([2, 1, 2, 3], 1, 2) -> [3]
```


### zip

Creates an array of elements, grouped based on the position in the original arrays.

Use `Math.max.apply()` to get the longest array in the arguments.
Creates an array with that length as return value and use `Array.from()` with a map-function to create an array of grouped elements.
If lengths of the argument-arrays vary, `undefined` is used where no value could be found.

```js
const zip = (...arrays) => {
  const maxLength = Math.max(...arrays.map(x => x.length));
  return Array.from({length: maxLength}).map((_, i) => {
   return Array.from({length: arrays.length}, (_, k) => arrays[k][i]);
  })
}
//zip(['a', 'b'], [1, 2], [true, false]); -> [['a', 1, true], ['b', 2, false]]
//zip(['a'], [1, 2], [true, false]); -> [['a', 1, true], [undefined, 2, false]]
```


### zipObject

Given an array of valid property identifiers and an array of values, return an object associating the properties to the values.

Since an object can have undefined values but not undefined property pointers, the array of properties is used to decide the structure of the resulting object using `Array.reduce()`.

```js
const zipObject = ( props, values ) => props.reduce( ( obj, prop, index ) => ( obj[prop] = values[index], obj ), {} )
// zipObject(['a','b','c'], [1,2]) -> {a: 1, b: 2, c: undefined}
// zipObject(['a','b'], [1,2,3]) -> {a: 1, b: 2}
```
