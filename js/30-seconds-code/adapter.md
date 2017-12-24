##Adapter

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

Changes a function that accepts an array into a variadic function.

Given a function, return a closure that collects all inputs into an array-accepting function.

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

Flip takes a function as an argument, then makes the first argument the last

Return a closure that takes variadic inputs, and splices the last argument to make it the first argument before applying the rest.

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

### promisify

Converts an asynchronous function to return a promise.

Use currying to return a function returning a `Promise` that calls the original function.
Use the `...rest` operator to pass in all the parameters.

*In Node 8+, you can use [`util.promisify`](https://nodejs.org/api/util.html#util_util_promisify_original)*

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

Takes a variadic function and returns a closure that accepts an array of arguments to map to the inputs of the function.

Use closures and the spread operator (`...`) to map the array of arguments to the inputs of the function.

```js
const spreadOver = fn => argsArr => fn(...argsArr);
/*
const arrayMax = spreadOver(Math.max)
arrayMax([1,2,3]) // -> 3
arrayMax([1,2,4]) // -> 4
*/
```
