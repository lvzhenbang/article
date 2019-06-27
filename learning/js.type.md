# javascript 常见判断

对象的字符串表示形式。

```
var _toString = Object.prototype.toString;
```

## isString

判断给定的值是否为` 字符串 `。如果是返回true，否则返回false。

```
function isString(val) {
  return typeof val === 'string';
}
```

## isNumber

判断给定的值是否为` 数字 `。如果是返回true，否则返回false。

```
function isNumber(val) {
  return typeof val === 'number';
}
```

### isFiniteNum

判断给定的值是否为` 有限数 `。如果是返回true，否则返回false。

```
function isFiniteNum(val) {
  return isNumber(val) && isFinite(val);
}
```

## isBoolean

判断给定的值是否为` boolean `。如果是返回true，否则返回false。

```
function isBoolean(val) {
  return typeof val === 'boolean';
}
```

## isUndefined

判断给定的值是否为` undefined `。如果是返回true，否则返回false。

```
function isUndefined(val) {
  return typeof val === 'undefined';
}
```

## isNull

判断给定的值是否为` null `。如果是返回true，否则返回false。

```
function isNull(val) {
  return (typeof val === 'object') && (String(val) === 'null');
}
```


## isSymbol

判断给定的值是否为` symbol `。如果是返回true，否则返回false。

```
function isSymbol(val) {
  return typeof val === 'symbol';
}
```

## isObject

判断给定的值是否为` object `。如果是返回true，否则返回false。

```
function isObject(val) {
  return val !== null && typeof val === 'object';
}
```

## isArray

判断给定的值是否为` 数组 `。如果是返回true，否则返回false。

```
function isArray(val) {
  return _toString.call(val) === '[object Array]';
}

```

### isArrayLike

判断给定的值是否为` 类数组 `。如果是返回true，否则返回false。

```
function isArrayLike(val) {
  return typeof val.length === 'number' && typeof val !== 'function';
}

```

### isTypedArray

判断给定的值是否为[` TypedArray `](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/TypedArray)。如果是返回true，否则返回false。


```
function isTypedArray(val) {
  return /^\[object (?:Float(?:32|64)|(?:Int|Uint)(?:8|16|32)|Uint8Clamped)Array\]$/.test(_toString.call(val));
}

```

## isFunction

判断给定的值是否为` funciton `。如果是返回true，否则返回false。

```
function isFunction(val) {
  return isSyncFunction(val) || isAsyncFunction(val);
}
```

### isSyncFunction

判断给定的值是否为` 同步funciton `。如果是返回true，否则返回false。

```
function isSyncFunction(val) {
  return _toString.call(val) === '[object Function]';
}
```

### isAsyncFunction

判断给定的值是否为` 异步funciton `。如果是返回true，否则返回false。

```
function isAsyncFunction(val) {
  return _toString.call(val) === '[object AsyncFunction]';
}
```

### isGeneratorFunction

判断给定的值是否为` 生成器funciton `。如果是返回true，否则返回false。

```
function isGeneratorFunction(val) {
	if (typeof val !== 'function') {
		return false;
	}

	return (val.constructor && val.constructor.name === 'GeneratorFunction') ||
		_toString.call(val) === '[object GeneratorFunction]';
}
```

注：参考[` is-generator-fn `](https://github.com/sindresorhus/is-generator-fn)

## isArguments

判断给定的值是否为` Arguments 对象 `。如果是返回true，否则返回false。

```
function isArguments(val) {
	return _toString.call(val) === '[object Arguments]'
}
```

## isDate

判断给定的值是否为` date `。如果是返回true，否则返回false。

```
function isDate(val) {
  return _toString.call(val) === '[object Date]';
}
```

## isRegExp

判断给定的值是否为` 正则 `。如果是返回true，否则返回false。

```
function isRegExp(val) {
  return _toString.call(val) === '[object RegExp]';
}
```

## isSet

判断给定的值是否为` Set `。如果是返回true，否则返回false。

```
function isSet(val) {
  return _toString.call(val) === '[object Set]';
}
```

## isWeakSet

判断给定的值是否为` WeakSet `。如果是返回true，否则返回false。

```
function isSeakSet(val) {
  return _toString.call(val) === '[object WeakSet]';
}
```

## isMap

判断给定的值是否为` Map `。如果是返回true，否则返回false。

```
function isMap(val) {
  return _toString.call(val) === '[object Map]';
}
```

## isWeakMap

判断给定的值是否为` Map `。如果是返回true，否则返回false。

```
function isWeakMap(val) {
  return _toString.call(val) === '[object WeakMap]';
}
```

## 其他

### isPromise

判断给定的值是否为` promise `。如果是返回true，否则返回false。

```
function isPromise(val) {
  return (
    (typeof val === 'object' || typeof val === 'function') &&
    typeof val.then === 'function' &&
    typeof val.catch === 'function');
}
```

注：参考[` is-promise `](https://github.com/then/is-promise/blob/master/index.js) 和 [` vue isPromise `](https://github.com/vuejs/vue/blob/dev/src/shared/util.js#L74)

### isFile

判断给定的值是否为` file `。如果是返回true，否则返回false。

```
function isFile(val) {
  return _toString.call(val) === '[object File]';
}
```

### isBlob

判断给定的值是否为` Blob `。如果是返回true，否则返回false。

```
function isBlob(val) {
  return _toString.call(val) === '[object Blob]';
}
```

### isStream

判断给定的值是否为` stream `。如果是返回true，否则返回false。

```
function isStream(val) {
  return isObject(val) && isFunction(val.pipe);
}
```

### isArrayBuffer

判断给定的值是否为` ArrayBuffer `。如果是返回true，否则返回false。

```
function isArrayBuffer(val) {
  return _toString.call(val) === '[object ArrayBuffer]';
}
```

### isArrayBufferView

判断给定的值是否为` ArrayBufferView `。如果是返回true，否则返回false。

```
function isArrayBufferView(val) {
  var result;
  if ((typeof ArrayBuffer !== 'undefined') && (ArrayBuffer.isView)) {
    result = ArrayBuffer.isView(val);
  } else {
    result = (val) && (val.buffer) && (val.buffer instanceof ArrayBuffer);
  }
  return result;
}
```

### isURLSearchParams

判断给定的值是否为` URLSearchParams `。如果是返回true，否则返回false。

```
function isURLSearchParams(val) {
  return typeof URLSearchParams !== 'undefined' && val instanceof URLSearchParams;
}
```

### isFormData

判断给定的值是否为` FormData `。如果是返回true，否则返回false。

```
function isFormData(val) {
  return (typeof FormData !== 'undefined') && (val instanceof FormData);
}
```

## 参考规范

[` ECMAScript® 2016 Language Specification `](http://www.ecma-international.org/ecma-262/7.0/#sec-ecmascript-language-types)
