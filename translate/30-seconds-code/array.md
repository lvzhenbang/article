
## Array

### 数组最大公约数 (arrayGcd)

计算数字数组最大公约数 (gcd).

用 `Array.reduce()` 和 `gcd` 运算式 (使用递归) 计算一个数字数组的最大公约数.

```js
const arrayGcd = arr => {
  const gcd = (x, y) => !y ? x : gcd(y, x % y);
  return arr.reduce((a,b) => gcd(a,b));
}
// arrayGcd([1,2,3,4,5]) -> 1
// arrayGcd([4,8,12]) -> 4
```

### 数组最小公倍数 (arrayLcm)

求数字数组的最小公倍数 (lcm).

用 `Array.reduce()` 和 `lcm` 运算式 (使用递归) 计算一个数字数组的最小公倍数.

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

过滤移除数组中重复出现的元素.

用 `Array.filter()` 保留数组中独一无二的元素.

```js
const filterNonUnique = arr => arr.filter(i => arr.indexOf(i) === arr.lastIndexOf(i));
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

过滤出指定索引的数组元素，并修改原数组.

用 `Array.filter()` 和 `Array.includes()` 提取不需要的元素.
用 `Array.length = 0` 初始化数组且长度为零, 用 `Array.push()` 重新传入剩余的元素.
用 `Array.push()` 记录pulled值 

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

从原数组中过滤出指定元素. 返回过滤出的元素.

用 `Array.filter()` 和 `Array.includes()` 移除不必要的元素.
用 `Array.length = 0` 初始化元素组且长度为零，用 `Array.push()` 重新传入剩余的元素.
用 `Array.push()` 记录 pulled 值 

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

```js（自定义）
const pullAtValue = (arr, pullArr) => {
  let removed = []; 
  arr.forEach((v, i) => pullArr.includes(v) ? removed.push(v) : v);
  pullArr.filter((v) => arr.splice(arr.indexOf(v), 1));
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

如果给定的函数返回 `false`, 则从数组中移除元素.

用 `Array.filter()` 找返回 `false` 的数组元素，用 `Array.reduce()` 和 `Array.splice()` 对原数组进行处理.
`func` 调用 (`value, index, array`) 参数.

```js
const remove = (arr, func) =>
  Array.isArray(arr) ? arr.filter(func).reduce((acc, val) => {
    arr.splice(arr.indexOf(val), 1);
    return acc.concat(val);
  }, []) : [];
// remove([1, 2, 3, 4], n => n % 2 == 0) -> [2, 4]
```


### sample

从数组中返回一个随机元素.

用 `Math.random()` 生成一个随机数, 乘以数组的 `length` ，然后 `Math.floor()` 进行下舍入.
这个方法也适合字符串.

```js
const sample = arr => arr[Math.floor(Math.random() * arr.length)];
// sample([3, 7, 9, 11]) -> 9
```

### similarity

返回一个数组，数组元素在两个给定的数组中均有.

用 `filter()`过滤出不在另一个数组中的 `values`, 确定条件用 `includes()`.

```js
const similarity = (arr, values) => arr.filter(v => values.includes(v));
// similarity([1,2,3], [1,2,4]) -> [1,2]
```


### symmetricDifference

返回一个数组，包含在给定的两个数组中均未出现的元素.

为每个数组创建一个 `Set` , 然后用 `Array.filter()` 过滤剩下都有的元素.

```js
const symmetricDifference = (a, b) => {
  const sA = new Set(a), sB = new Set(b);
  return [...a.filter(x => !sB.has(x)), ...b.filter(x => !sA.has(x))];
}
// symmetricDifference([1,2,3], [1,2,4]) -> [3,4]
```

```js(自定义)
const symmetricDifference = (a, b) => [...(new Set([...a,...b]))].filter((v) => !a.includes(v) || !b.includes(v));
// symmetricDifference([1,2,3], [1,2,4]) -> [3,4]
```

### shuffle

随机化一个数组值的顺序.

用 `Array.sort()` 重新排序数组 , 用 `Math.random()` 作比较器.

```js
const shuffle = arr => arr.sort(() => Math.random() - 0.5);
// shuffle([1,2,3]) -> [2,3,1]
```


### tail

返回数组中出第一个元素外的所有元素.

如果，数组 `length` 大于 `1` 返回 `arr.slice(1)`，否者，返回整个数组.

```js
const tail = arr => arr.length > 1 ? arr.slice(1) : arr;
// tail([1,2,3]) -> [2,3]
// tail([1]) -> [1]
```


### take

返回指定长度的数组，数组中元素从数组第一个元素起.

用 `Array.slice()` 从原始数组的第一个元素起，截取 `n` 个元素.

```js
const take = (arr, n = 1) => arr.slice(0, n);
// take([1, 2, 3], 5) -> [1, 2, 3]
// take([1, 2, 3], 0) -> []
```


### takeRight

返回指定长度的数组，数组中元素从数组最后一个元素起.

用 `Array.slice()` 从原始数组的最后一个元素起，截取 `n` 个元素.

```js
const takeRight = (arr, n = 1) => arr.slice(arr.length - n, arr.length);
// takeRight([1, 2, 3], 2) -> [ 2, 3 ]
// takeRight([1, 2, 3]) -> [3]
```


### union

返回两个数组中存在的所有元素.

创建一个 `Set` 用 `a` 和 `b` 所有元素，然后转换为数组.

```js
const union = (a, b) => Array.from(new Set([...a, ...b]));
// union([1,2,3], [4,3,2]) -> [1,2,3,4]
```


### without

从数组中移除所有指定的元素.

用 `Array.filter()` 过滤出所有制定的元素(用 `!Array.includes()` 做过滤条件 ).

_(For a snippet that mutates the original array see [`pull`](#pull))_

```js
const without = (arr, ...args) => arr.filter(v => !args.includes(v));
// without([2, 1, 2, 3], 1, 2) -> [3]
```


### zip

创建一个数组，根据原始数组中的位置进行分组.

用 `Math.max.apply()` 获得参数中最长的数组的长度.
创建一个用该长度做返回值并使用 `array.from` 和 `map()` 创建分组的元素数组.
如果参数数组的长度不同, `undefined` 将被用于没有元素的地方.

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

给定一个有效的属性标识数组，返回一个属性与值相关联的对象.
因为对象可以有未定义的值，但不能有未定义的属性指针，所以使用 `Array.reduce()` 决定结果对象的结构.

```js
const zipObject = ( props, values ) => props.reduce( ( obj, prop, index ) => ( obj[prop] = values[index], obj ), {} )
// zipObject(['a','b','c'], [1,2]) -> {a: 1, b: 2, c: undefined}
// zipObject(['a','b'], [1,2,3]) -> {a: 1, b: 2}
```
