## Adapter

### call

给定一个键值和一组参数，但给定一个上下文时调用它们。

使用闭包调用存储的键值与存储的参数  

```js
const call = ( key, ...args ) => context => context[ key ]( ...args );
/*
Promise.resolve( [ 1, 2, 3 ] ).then( call('map', x => 2 * x ) ).then( console.log ) //[ 2, 4, 6 ]
const map = call.bind(null, 'map')
Promise.resolve( [ 1, 2, 3 ] ).then( map( x => 2 * x ) ).then( console.log ) //[ 2, 4, 6 ]
*/
```

### collectInto

改变一个可以接受数组为参数的函数成为可变量函数.

给定一个函数，它返回一个把所有传入的参数转变为数组的闭包函数.

```js
const collectInto = fn => ( ...args ) => fn( args );
/*
const Pall = collectInto( Promise.all.bind(Promise) )
let p1 = Promise.resolve(1)
let p2 = Promise.resolve(2)
let p3 = new Promise((resolve) => setTimeout(resolve,2000,3))
Pall(p1, p2, p3).then(console.log)
*/
```

### flip

`Flip` 利用一个函数作为参数，然后降低一个参数转变为最后一个参数

用一个闭包接受可变参数的输入, 在使用出第一个参数外的其余参数前先拼接第一个参数使它成为最后一个参数.

```js
const flip = fn => (...args) => fn(args.pop(), ...args)
/*
let a = {name: 'John Smith'}
let b = {}
const mergeFrom = flip(Object.assign)
let mergePerson = mergeFrom.bind(a)
mergePerson(b) // == b
b = {}
Object.assign(b, a) // == b
*/
```

### pipeFunctions

从左到右的执行函数组合.

用 `Array.reduce()` 和 spread (...) 操作符来实现从左向右的执行函数组合. 第一个函数可以接受任意个参数，其余的只能接受一个参数.

```
const pipeFunctions = (...fns) => fns.reduce((f, g) => (...args) => g(f(...args)));
/*
const add5 = x => x + 5
const multiply = (x, y) => x * y
const multiplyAndAdd5 = pipeFunctions(multiply, add5)
multiplyAndAdd5(5, 2) -> 15
*/
```

### promisify

转化一个返回 promise 的异步函数.

返回一个函数，它返回一个调用所有原始函数的 `Promise` .
用 `...rest` 去传递输入的参数.

*在 Node 8+ 中, 你可以用 [`util.promisify`](https://nodejs.org/api/util.html#util_util_promisify_original)*

```js
const promisify = func =>
  (...args) =>
    new Promise((resolve, reject) =>
      func(...args, (err, result) =>
        err ? reject(err) : resolve(result))
    );
// const delay = promisify((d, cb) => setTimeout(cb, d))
// delay(2000).then(() => console.log('Hi!')) -> Promise resolves after 2s
```


### spreadOver

用一个可变参数的函数并返回一个闭包，该闭包可以将输入的数组参数的转变为参数序列.

用闭包和spread (`...`) 操作符去映射一个函数的参数数组.

```js
const spreadOver = fn => argsArr => fn(...argsArr);
/*
const arrayMax = spreadOver(Math.max)
arrayMax([1,2,3]) // -> 3
arrayMax([1,2,4]) // -> 4
*/
```
