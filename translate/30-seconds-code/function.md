## Function

### chainAsync

异步执行函数链.

循环一个包含异步事件的函数数组, 当异步事件执行完毕后调用 `next` 方法.

```js
const chainAsync = fns => { let curr = 0; const next = () => fns[curr++](next); next(); };
/*
chainAsync([
  next => { console.log('0 seconds'); setTimeout(next, 1000); },
  next => { console.log('1 second');  setTimeout(next, 1000); },
  next => { console.log('2 seconds'); }
])
*/
```

### curry

函数的柯里化.

用递归.
如果传入的参数对象 (`args`) 是有效的, 然后调用传入的函数 `fn`.
否则返回 `fn` 和剩余参数中它所需的参数.
如果想要柯里化一个接受任意参数的函数(可变参数, e.g. `Math.min()`), 你需要传入第二个参数 `arity`.

```js
const curry = (fn, arity = fn.length, ...args) =>
  arity <= args.length
    ? fn(...args)
    : curry.bind(null, fn, arity, ...args);
// curry(Math.pow)(2)(10) -> 1024
// curry(Math.min, 3)(10)(50)(2) -> 2
```

### functionName

记录函数的名字.

用 `console.debug()` 和 被传递的方法的 `name` 属性来记录方法的名字并打印在控制台上.

```js
const functionName = fn => (console.debug(fn.name), fn);
// functionName(Math.max) -> max (logged in debug channel of console)
```


### pipe

按照 left-to-right 的顺序执行函数的集合.

用 `Array.reduce()` 和 (`...`) 操作符按照 left-to-right 的顺序执行函数的集合.
第一个函数可以接受一个或者更多的参数; 其余的函数必须是一元函数.

```js
const pipeFunctions = (...fns) => fns.reduce((f, g) => (...args) => g(f(...args)));
/*
const add5 = x => x + 5
const multiply = (x, y) => x * y
const multiplyAndAdd5 = pipeFunctions(multiply, add5)
multiplyAndAdd5(5, 2) -> 15
*/
```


### compose

按照 right-to-left 的顺序执行函数的集合.

用 `Array.reduce()` to perform right-to-left function composition.
最后一个函数可以接受一个或者更多的参数; 其余的函数必须是一元函数.

```js
const compose = (...fns) => fns.reduce((f, g) => (...args) => f(g(...args)));
/*
const add5 = x => x + 5
const multiply = (x, y) => x * y
const multiplyAndAdd5 = compose(add5, multiply)
multiplyAndAdd5(5, 2) -> 15
*/
```


### runPromisesInSeries

执行一组串联的promise.

用 `Array.reduce()` 创建一个promise链, 链中每一个promise状态变为resolved，返回下一个promise.

```js
const runPromisesInSeries = ps => ps.reduce((p, next) => p.then(next), Promise.resolve());
// const delay = (d) => new Promise(r => setTimeout(r, d))
// runPromisesInSeries([() => delay(1000), () => delay(2000)]) -> 按顺序的执行promise链，共花费3s的时间
```


### sleep

延迟异步函数的执行.

`async` 函数的延迟功能部分的实现是通过将该异步函数放入定时器中，然后返回一个 `Promise`.

```js
const sleep = ms => new Promise(resolve => setTimeout(resolve, ms));
/*
async function sleepyWork() {
  console.log('I\'m going to sleep for 1 second.');
  await sleep(1000);
  console.log('I woke up after 1 second.');
}
*/
```
