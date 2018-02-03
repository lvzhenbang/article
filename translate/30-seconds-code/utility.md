## Utility

文章来源于：[https://github.com/Chalarangelo/30-seconds-of-code/blob/master/README.md](https://github.com/Chalarangelo/30-seconds-of-code/blob/master/README.md)

### coalesce

返回参数列表中第一个非null和undefined的参数.

用 `Array.find()` 实现返回第一个非 `null`/`undefined` 参数.

```js
const coalesce = (...args) => args.find(_ => ![undefined, null].includes(_))
// coalesce(null,undefined,"",NaN, "Waldo") -> ""
```


### coalesceFactory

返回一个`customCoalesce`, 用自定义函数中的validate函数是否返回true来对`customCoalesce`中传入的参数列表尽心过滤.

用 `Array.find()`去遍历参数列表，用给定的validate函数的返回值来过滤参数列表.

```js
const coalesceFactory = valid => (...args) => args.find(valid);
// const customCoalesce = coalesceFactory(_ => ![null, undefined, "", NaN].includes(_))
// customCoalesce(undefined, null, NaN, "", "Waldo") //-> "Waldo"
```

### getType

返回给定值的基本类型.

返回给定值的构造函数名字的小写形式, 如果是"undefined" 或 "null"，返回"undefined"或"null"

```js
const getType = v =>
  v === undefined ? 'undefined' : v === null ? 'null' : v.constructor.name.toLowerCase();
// getType(new Set([1,2,3])) -> "set"
```


### extendHex

将一个3位的HEX颜色值转化为6位的.

用 `Array.map()`, `split()` 和 `Array.join()` 来实现.
如果HEX是以 `#`开始的，用`String.slice()`返回一个没有该字符的字符串.

```js
const extendHex = shortHex =>
  '#' + shortHex.slice(shortHex.startsWith('#') ? 1 : 0).split('').map(x => x+x).join('')
// extendHex('#03f') -> '#0033ff'
// extendHex('05a') -> '#0055aa'
```


### hexToRGB

转换HEX颜色为 `rgb()` ，如果alpha提供的化,转化为 `rgba()` .

用按位右移操作符(>>>)和掩码(&)操作符去转换3位进制的颜色为RGB颜色. 

如果是三位进制的就先转化为标准的6位的. 如果在6位后指定 `alpha` 值, 则返回 `rgba()` .

```js
const hexToRGB = hex => {
  let alpha = false, h = hex.slice(hex.startsWith('#') ? 1 : 0);
  if (h.length === 3) h = [...h].map(x => x + x).join('');
  else if (h.length === 8) alpha = true;
  h = parseInt(h, 16);
  return 'rgb' + (alpha ? 'a' : '') + '('
    + (h >>> (alpha ? 24 : 16)) + ', '
    + ((h & (alpha ? 0x00ff0000 : 0x00ff00)) >>> (alpha ? 16 : 8)) + ', '
    + ((h & (alpha ? 0x0000ff00 : 0x0000ff)) >>> (alpha ? 8 : 0))
    + (alpha ? `, ${(h & 0x000000ff)}` : '') + ')';
};
// hexToRGB('#27ae60ff') -> 'rgba(39, 174, 96, 255)'
// hexToRGB('27ae60') -> 'rgb(39, 174, 96)'
// hexToRGB('#fff') -> 'rgb(255, 255, 255)'

```


### RGBToHex

将RGB颜色转换为HEX颜色.

将RGB的颜色用操作符 (`<<`) 和 `toString(16)` 转换16进制的字符串, 然后 `padStart(6,'0')` 得到一个6位的HEX颜色

`padStart()` 是ES6中的新方法，定义字符串的格式，第一个参数是设定字符串的长度，第二个参数指定不足位置用什么来代替

```js
const RGBToHex = (r, g, b) => ((r << 16) + (g << 8) + b).toString(16).padStart(6, '0');
// RGBToHex(255, 165, 1) -> 'ffa501'
```


### isArray

判断给定给定的值是否是否是数组.

用 `Array.isArray()` 判断给定的值是否为一个数组.

```js
const isArray = val => !!val && Array.isArray(val);
// isArray(null) -> false
// isArray([1]) -> true
```


### isBoolean

半段给定的值是否是一个基本的Boolean值.

用 `typeof` 判断一个值是否为一个基本的Boolean值.

```js
const isBoolean = val => typeof val === 'boolean';
// isBoolean(null) -> false
// isBoolean(false) -> true
```


### isFunction

检查给定的值是否为一个函数.

用 `typeof` 判断给定的值是否为一个基本的函数类型.

```js
const isFunction = val => val && typeof val === 'function';
// isFunction('x') -> false
// isFunction(x => x) -> true
```


### isNumber

判断给定的值是否为一个数字类型.

用 `typeof` 判断一个值是否为基本的数字类型.

```js
const isNumber = val => typeof val === 'number';
// isNumber('1') -> false
// isNumber(1) -> true
```


### isString

判断给定的值是否为一个字符串.

用 `typeof` 判断给定的值是否为一个基本的字符串类型.

```js
const isString = val => typeof val === 'string';
// isString(10) -> false
// isString('10') -> true
```


### isSymbol

判断给定的值是否为symbol类型.

用 `typeof` 判断给定的值是否为基本的symbol类型.

```js
const isSymbol = val => typeof val === 'symbol';
// isSymbol('x') -> false
// isSymbol(Symbol('x')) -> true
```


### timeTaken

计算函数执行的时间.

用 `console.time()` 和 `console.timeEnd()` 去计算从函数开始执行到执行结束所花费的时间.

```js
const timeTaken = callback => {
  console.time('timeTaken');  const r = callback();
  console.timeEnd('timeTaken');  return r;
};
// timeTaken(() => Math.pow(2, 10)) -> 1024
// (logged): timeTaken: 0.02099609375ms
```


### toDecimalMark

用 `toLocaleString()` 去转换浮点数，用逗号将数字分割成字符串的格式 [Decimal mark](https://en.wikipedia.org/wiki/Decimal_mark).

```js
const toDecimalMark = num => num.toLocaleString("en-US");
// toDecimalMark(12305030388.9087) -> "12,305,030,388.9087"
```


### toOrdinalSuffix

为数字添加序号后缀.

用模操作符 (`%`) 取得指定值的个位和十分位的数值.然后找到相对应的序号与之匹配.
如果`digit`上没有找到，就用 `tpattern`

```js
const toOrdinalSuffix = num => {
  const int = parseInt(num), digits = [(int % 10), (int % 100)],
    ordinals = ['st', 'nd', 'rd', 'th'], oPattern = [1, 2, 3, 4],
    tPattern = [11, 12, 13, 14, 15, 16, 17, 18, 19];
  return oPattern.includes(digits[0]) && !tPattern.includes(digits[1]) ? int + ordinals[digits[0] - 1] : int + ordinals[3];
};
// toOrdinalSuffix("123") -> "123rd"
```


### UUIDGenerator

生成 UUID.

用 `crypto` API生成UUID, 参考资料 [RFC4122](https://www.ietf.org/rfc/rfc4122.txt) version 4.

```js
const UUIDGenerator = () =>
  ([1e7] + -1e3 + -4e3 + -8e3 + -1e11).replace(/[018]/g, c =>
    (c ^ crypto.getRandomValues(new Uint8Array(1))[0] & 15 >> c / 4).toString(16)
  );
// UUIDGenerator() -> '7982fcfe-5721-4632-bede-6000885be57d'
```


### validateNumber

如果给定的值是数字类型，那么返回 `true`; 否则 `false`.

用 `!isNaN` 结合 `parseFloat()` 来判定给定的值是否是数字类型.
用 `isFinite()` 数字的长度是否有限.
用 `Number()` 去进行是否为数字的强迫检查.

```js
const validateNumber = n => !isNaN(parseFloat(n)) && isFinite(n) && Number(n) == n;
// validateNumber('10') -> true
```
