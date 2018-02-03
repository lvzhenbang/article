## Object

### cleanObj

删除JSON对象中除指定键值的属性.

用递归的方法
用 `Object.keys()` 方法遍历JSON对象然后删除不是`include`在给定数组中的属性.
如果你传入 `childIndicator` ，它将对该键所对应的JSON对象进行深度遍历.

```js
const cleanObj = (obj, keysToKeep = [], childIndicator) => {
  Object.keys(obj).forEach(key => {
    if (key === childIndicator) {
      cleanObj(obj[key], keysToKeep, childIndicator);
    } else if (!keysToKeep.includes(key)) {
      delete obj[key];
    }
  })
}
/*
  const testObj = {a: 1, b: 2, children: {a: 1, b: 2}}
  cleanObj(testObj, ["a"],"children")
  console.log(testObj)// { a: 1, children : { a: 1}}
*/
```

### cleanObj的变形（非原著作）

对所有的键对应的JSON对象进行深度遍历

用 `Object.keys()` 方法遍历JSON对象然后删除不是 `include` 在给定数组中的属性.
如果给定的属性值为object，则看传入的 `dep` 是否为false，或者没有第三个参数，则直接删除.
如果传入 `dep` 为true，则进行深度遍历.
```
const cleanObj = (obj, keysToKeep = [], dep=false) => {
  Object.keys(obj).forEach(key => {
    if(dep) {
      if (obj[key].constructor.name === 'Object') {
        cleanObj(obj[key], keysToKeep, dep)
      } else {
        if (!keysToKeep.includes(key)) {
          delete obj[key];
        }
      }
    } else {
      if (!keysToKeep.includes(key)) {
        delete obj[key];
      }
    }
  })
  return obj
}
/*
  const testObj = {a:1, b:{a:1, b:2, c:3}, c:{a:1, b:2}, d: {a:1, b:2, c:3}}
  cleanObj(testObj, ["a"],"children")  // {a: 1, b: {a: 1}, c: {a: 1}, d: {a: 1, c: 3}}
*/
```

### objectFromPairs

将一个键值对形式的数组转化为对象
用 `Array.reduce()` 去创建对象并合并键值对.

```js
const objectFromPairs = arr => arr.reduce((a, v) => (a[v[0]] = v[1], a), {});
// objectFromPairs([['a',1],['b',2]]) -> {a: 1, b: 2}
```


### objectToPairs

将一个对象转化为键值对形式的数组.

用 `Object.keys()` 将对象转化为对象的键数组，然后 `Array.map()` 生成一个键值对的数组.

```js
const objectToPairs = obj => Object.keys(obj).map(k => [k, obj[k]]);
// objectToPairs({a: 1, b: 2}) -> [['a',1],['b',2]])
```


### orderBy

按对象属性和排序规则排序对象数组.

使用 `Array.sort()` 的自定义排序规则排序,根据传入的 `order`排序方式，用解构来实现指定的属性的位置交换.
如果没有传入参数 `order` 默认值为asc.

```js
const orderBy = (arr, props, orders) =>
  arr.sort((a, b) =>
    props.reduce((acc, prop, i) => {
      if (acc === 0) {
        const [p1, p2] = orders && orders[i] === 'desc' ? [b[prop], a[prop]] : [a[prop], b[prop]];
        acc = p1 > p2 ? 1 : p1 < p2 ? -1 : 0;
      }
      return acc;
    }, 0)
  );
/*
const users = [{ 'name': 'fred',   'age': 48 },{ 'name': 'barney', 'age': 36 },
  { 'name': 'fred',   'age': 40 },{ 'name': 'barney', 'age': 34 }];
orderby(users, ['name', 'age'], ['asc', 'desc']) -> [{name: 'barney', age: 36}, {name: 'barney', age: 34}, {name: 'fred', age: 48}, {name: 'fred', age: 40}]
orderby(users, ['name', 'age']) -> [{name: 'barney', age: 34}, {name: 'barney', age: 36}, {name: 'fred', age: 40}, {name: 'fred', age: 48}]
*/
```


### select

从一个对象中检索出选择其所代表的属性.

如果属性不存在返回 `undefined`.

```js
const select = (from, selector) =>
  selector.split('.').reduce((prev, cur) => prev && prev[cur], from);

// const obj = {selector: {to: {val: 'val to select'}}};
// select(obj, 'selector.to.val'); -> 'val to select'
```


### shallowClone

对象的浅拷贝.

用 `Object.assign()` 和一个 (`{}`) 来实现对源对象的浅拷贝.

```js
const shallowClone = obj => Object.assign({}, obj);
/*
const a = { x: true, y: 1 };
const b = shallowClone(a);
a === b -> false
*/
```


### truthCheckCollection

判断第一个参数所代表的集合中的每一个对象中是否包含第二个参数所代表的属性.

用 `Array.every()` 去检查结合中每一个对象是否包含`pre`属性.
 
 ```js
const truthCheckCollection = (collection, pre) => (collection.every(obj => obj[pre]));
// truthCheckCollection([{"user": "Tinky-Winky", "sex": "male"}, {"user": "Dipsy", "sex": "male"}], "sex") -> true
```