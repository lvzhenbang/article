## Object

### cleanObj

Removes any properties except the ones specified from a JSON object.

Use `Object.keys()` method to loop over given JSON object and deleting keys that are not `include`d in given array.
Also if you give it a special key (`childIndicator`) it will search deeply inside it to apply function to inner objects too.

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


### objectFromPairs

Creates an object from the given key-value pairs.

Use `Array.reduce()` to create and combine key-value pairs.

```js
const objectFromPairs = arr => arr.reduce((a, v) => (a[v[0]] = v[1], a), {});
// objectFromPairs([['a',1],['b',2]]) -> {a: 1, b: 2}
```


### objectToPairs

Creates an array of key-value pair arrays from an object.

Use `Object.keys()` and `Array.map()` to iterate over the object's keys and produce an array with key-value pairs.

```js
const objectToPairs = obj => Object.keys(obj).map(k => [k, obj[k]]);
// objectToPairs({a: 1, b: 2}) -> [['a',1],['b',2]])
```


### orderBy

Returns a sorted array of objects ordered by properties and orders.

Uses a custom implementation of sort, that reduces the props array argument with a default value of 0, it uses destructuring to swap the properties position depending on the order passed.
If no orders array is passed it sort by 'asc' by default.

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

Retrieve a property that indicated by the selector from an object.

If the property does not exists returns `undefined`.

```js
const select = (from, selector) =>
  selector.split('.').reduce((prev, cur) => prev && prev[cur], from);

// const obj = {selector: {to: {val: 'val to select'}}};
// select(obj, 'selector.to.val'); -> 'val to select'
```


### shallowClone

Creates a shallow clone of an object.

Use `Object.assign()` and an empty object (`{}`) to create a shallow clone of the original.

```js
const shallowClone = obj => Object.assign({}, obj);
/*
const a = { x: true, y: 1 };
const b = shallowClone(a);
a === b -> false
*/
```


### truthCheckCollection

Checks if the predicate (second argument) is truthy on all elements of a collection (first argument).

Use `Array.every()` to check if each passed object has the specified property and if it returns a truthy value.
 
 ```js
const truthCheckCollection = (collection, pre) => (collection.every(obj => obj[pre]));
// truthCheckCollection([{"user": "Tinky-Winky", "sex": "male"}, {"user": "Dipsy", "sex": "male"}], "sex") -> true
```