# javascript 常见判断

对象的字符串表示形式。

```
var _toString = Object.prototype.toString;
```

## isString

判断给定的值` val `是否为` 字符串 `。如果是返回true，否则返回false。

```
function isString(val) {
  return typeof val === 'string';
}
```

## isNumber

判断给定的值` val `是否为` 数字 `。如果是返回true，否则返回false。

```
function isNumber(val) {
  return typeof val === 'number';
}
```

### isFiniteNum

判断给定的值` val `是否为` 有穷数 `。如果是返回true，否则返回false。

```
function isFiniteNum(val) {
  return isNumber(val) && isFinite(val);
}
```

注：ES6中已经实现，可以使用` Number `的静态方法` isFinite `

### isIntger

判断给定的值` val `是否为` 一个整数 `。如果是返回true，否则返回false。

```
function isInteger(val) {
  return !isObject(val) && isFiniteNum(val) && Math.floor(val) === val;
};
```

注：ES6中已经实现，可以使用` Number `的静态方法` isIntger `

### isNaN

判断给定的值` val `是否为` 非数字 `。如果是返回true，否则返回false。

```
function isNaN(val) {
  if (typeof val !== 'number') {
    return false;
  }

  if (window.isNaN(val)) {
    return true;
  }

  return false;
}
```
注：虽然在ES6中已经实现，可以使用` Number `的静态方法` isNaN `，但是它存在问题。如：` Number.isNaN(null) `返回` false `，` Number.isNaN(NaN) `返回` true `等这些异常错误。

## isBoolean

判断给定的值` val `是否为` boolean `。如果是返回true，否则返回false。

```
function isBoolean(val) {
  return typeof val === 'boolean';
}
```

## isUndefined

判断给定的值` val `是否为` undefined `。如果是返回true，否则返回false。

```
function isUndefined(val) {
  return typeof val === 'undefined';
}
```

## isNull

判断给定的值` val `是否为` null `。如果是返回true，否则返回false。

```
function isNull(val) {
  return (typeof val === 'object') && (String(val) === 'null');
}
```


## isSymbol

判断给定的值` val `是否为` symbol `。如果是返回true，否则返回false。

```
function isSymbol(val) {
  return typeof val === 'symbol';
}
```

## isObject

判断给定的值` val `是否为` object `。如果是返回true，否则返回false。

```
function isObject(val) {
  return val !== null && (typeof val === 'object' || typeof === 'function');
}
```

注：[` core-js `](https://github.com/zloirock/core-js/blob/master/packages/core-js/internals/is-object.js)


### isPlainObject

判断给定的值` val `是否为` 单纯的对象（object） `（不是 Array, Date等）。如果是返回true，否则返回false。

```
function isPlainObject(val) {
  return _toString.call(obj) === '[object Object]'
}
```

注：` _toString `可参考[` js.type _toString `](https://github.com/lvzhenbang/article/blob/master/js/check/js.type.md#javascript-%E5%B8%B8%E8%A7%81%E5%88%A4%E6%96%AD)

注：后期参考[` lodash isPlainObject `](https://github.com/lodash/lodash/blob/master/isPlainObject.js) 修改。

### isEmptyObject

判断给定的值` val `是否为` 空对象（{}） `。如果是返回true，否则返回false。

```
function isEmptyObject(val) {
	for (const key in val) {
		if (val.hasOwnProperty(key)) {
			return false
		}
	}
	return true
}
```

注：使用它之前需要用[` isObject(val) `](https://github.com/lvzhenbang/article/blob/master/js/check/js.type.md#isobject)判断给定的值` val `是否为对象


### hasProp

判断某个对象` obj `是否有` 属性key `。如果有返回true，否则返回false。

```
function hasProp(obj, key) {
  return isObject(obj) && hasOwnProperty.call(obj, key);
}
```

注：` isObject(obj) `参考[` js.type isObject `](https://github.com/lvzhenbang/article/blob/master/js/check/js.type.md#isobject)

## isArray

判断给定的值` val `是否为` 数组 `。如果是返回true，否则返回false。

```
function isArray(val) {
  return _toString.call(val) === '[object Array]';
}

```

### isArrayLike

判断给定的值` val `是否为` 类数组 `。如果是返回true，否则返回false。

```
function isArrayLike(val) {
  return isDef(val) && typeof val.length === 'number' && typeof val !== 'function';
}

```

注：` isDef(val) `参考[` type isDef `](https://github.com/lvzhenbang/article/blob/master/js/type/other.md#isdef)

### isTypedArray

判断给定的值` val `是否为[` TypedArray `](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/TypedArray)。如果是返回true，否则返回false。


```
function isTypedArray(val) {
  return /^\[object (?:Float(?:32|64)|(?:Int|Uint)(?:8|16|32)|Uint8Clamped)Array\]$/.test(_toString.call(val));
}

```

## isFunction

判断给定的值` val `是否为` funciton `。如果是返回true，否则返回false。

```
function isFunction(val) {
  return isSyncFunction(val) || isAsyncFunction(val);
}
```

### isSyncFunction

判断给定的值` val `是否为` 同步funciton `。如果是返回true，否则返回false。

```
function isSyncFunction(val) {
  return _toString.call(val) === '[object Function]';
}
```

### isAsyncFunction

判断给定的值` val `是否为` 异步funciton `。如果是返回true，否则返回false。

```
function isAsyncFunction(val) {
  return _toString.call(val) === '[object AsyncFunction]';
}
```

### isGeneratorFunction

判断给定的值` val `是否为` 生成器funciton `。如果是返回true，否则返回false。

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

判断给定的值` val `是否为` Arguments 对象 `。如果是返回true，否则返回false。

```
function isArguments(val) {
	return _toString.call(val) === '[object Arguments]'
}
```

## isRegExp

判断给定的值` val `是否为` 正则 `。如果是返回true，否则返回false。

```
function isRegExp(val) {
  return _toString.call(val) === '[object RegExp]';
}
```

## isSet

判断给定的值` val `是否为` Set `。如果是返回true，否则返回false。

```
function isSet(val) {
  return _toString.call(val) === '[object Set]';
}
```

## isWeakSet

判断给定的值` val `是否为` WeakSet `。如果是返回true，否则返回false。

```
function isSeakSet(val) {
  return _toString.call(val) === '[object WeakSet]';
}
```

## isMap

判断给定的值` val `是否为` Map `。如果是返回true，否则返回false。

```
function isMap(val) {
  return _toString.call(val) === '[object Map]';
}
```

## isWeakMap

判断给定的值` val `是否为` Map `。如果是返回true，否则返回false。

```
function isWeakMap(val) {
  return _toString.call(val) === '[object WeakMap]';
}
```

## isPromise

判断给定的值` val `是否为` promise `。如果是返回true，否则返回false。

```
function isPromise(val) {
  return (
    (typeof val === 'object' || typeof val === 'function') &&
    typeof val.then === 'function' &&
    typeof val.catch === 'function');
}
```

注：参考[` is-promise `](https://github.com/then/is-promise/blob/master/index.js) 和 [` vue isPromise `](https://github.com/vuejs/vue/blob/dev/src/shared/util.js#L74)


## isFormData

判断给定的值` val `是否为` FormData `。如果是返回true，否则返回false。

```
function isFormData(val) {
  return (typeof FormData !== 'undefined') && (val instanceof FormData);
}
```

## event

### isTouchEvent()

判断DOM触发的事件类型` event(e) `是否为` TouchEvent `。如果是返回true，否则返回false。

```
function isTouchEvent (e) {
  return e.constructor.name === 'TouchEvent'
}
```

# 参考规范

[` ECMAScript® 2016 Language Specification `](http://www.ecma-international.org/ecma-262/7.0/#sec-ecmascript-language-types)
