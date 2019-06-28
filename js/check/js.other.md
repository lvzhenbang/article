# js其他判断

## data type

### isDef

判断给定的值是否为` 已定义类型 `。如果是返回true，否则返回false。

```
function isDef(val) {
  return val !== undefined && val !== null
}
```

### isPrimitive

判断给定的值是否为` 基本类型 `。如果是返回true，否则返回false。

```
function isPrimitive(val) {
  return (
    typeof val === 'string' ||
    typeof val === 'number' ||
    typeof val === 'symbol' ||
    typeof val === 'boolean' ||
    typeof val === 'undefined' ||
    (isNull(val))
  )
}
```

注：`isNull(val)`可参考[` js.type isNull `](https://github.com/lvzhenbang/article/blob/master/js/check/js.type.md#isnull)

注：在vue中，把` null `和它衍生出的` undefined `分离了出来，可参考[`vue isPrimitive`](https://github.com/vuejs/vue/blob/dev/src/shared/util.js#L26)

注：[`初始类型`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#Data_types)

### isPlainObject

判断给定的值是否为` 单纯的对象（object） `（不是 Array, Date等）。如果是返回true，否则返回false。

```
function isPlainObject(val) {
  return _toString.call(obj) === '[object Object]'
}
```

注：` _toString `可参考[` js.type _toString `](https://github.com/lvzhenbang/article/blob/master/js/check/js.type.md#javascript-%E5%B8%B8%E8%A7%81%E5%88%A4%E6%96%AD)

注：后期参考[` lodash isPlainObject `](https://github.com/lodash/lodash/blob/master/isPlainObject.js) 修改。

### isEmptyObject

判断给定的值是否为` 空对象（{}） `。如果是返回true，否则返回false。

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

注：使用它之前需要用[` isObject(val) `](https://github.com/lvzhenbang/article/blob/master/js/check/js.type.md#isobject)判断给定的值是否为对象

### hasProp

判断某个对象` obj `是否有` 属性key `。如果有返回true，否则返回false。

```
function hasProp(obj, key) {
  return isObject(obj) && hasOwnProperty.call(obj, key);
}
```

注：` isObject(obj) `参考[` js.type isObject `](https://github.com/lvzhenbang/article/blob/master/js/check/js.type.md#isobject)

## isNative

判断一个方法` val `是否是一个原生的方法。如果是返回true，否则返回false。

```
var reIsNative = RegExp(`^${
  Function.prototype.toString.call(Object.prototype.hasOwnProperty)
    .replace(/[\\^$.*+?()[\]{}|]/g, '\\$&')
    .replace(/hasOwnProperty|(function).*?(?=\\\()| for .+?(?=\\\])/g, '$1.*?')
}$`);

function isNative(val) {
  return isFunction(val) && reIsNative.test(val)
}
```

注：` isFunction(val) `参考[` js.type isFunction `](https://github.com/lvzhenbang/article/blob/master/js/check/js.type.md#isfunction)

## string 相关

### isNonBlankString

判断给定的值是否为` 非空字符串 `。如果是返回true，否则返回false。

```
function isNonBlankString(val) {
  return typeof val === 'string' && (/\S/).test(val);
}
```

### isBlankString

判断给定的值是否为` 空字符串 `。如果是返回true，否则返回false。

```
function isBlankString(val) {
  return typeof val === 'string' && val.trim().length === 0;
}
```

## element

### isEl

判断给定的值是否为` dom元素 `。如果是返回true，否则返回false。

```
function isEl(val) {
  return isObject(val) && val.nodeType === 1;
}
```

注：`isObject(val)`，请参考[`js type`](https://github.com/lvzhenbang/article/blob/master/js/check/js.type.md#isobject)

### isTextNode

判断给定的值是否为` 文本节点 `。如果是返回true，否则返回false。

```
function isTextNode(val) {
  return isObject(val) && val.nodeType === 3;
}
```

注：`isObject(val)`，请参考[`js type`](https://github.com/lvzhenbang/article/blob/master/js/check/js.type.md#isobject)


## iframe

### isInframe

判断给定的值是否为` Inframe元素 `。如果是返回true，否则返回false。

```
function isInFrame() {
  try {
    return window.parent !== window.self;
  } catch (x) {
    return true;
  }
}
```

注：在Safari中，如果获取`parent` 或 `self`，将抛出异常，因此需要使用`try/catch`。

## mouse

### isSingleLeftClick

判断是否为`鼠标左键单击`。如果是返回true，否则返回false。

```
function isSingleLeftClick(event) {
  if (event.button === undefined && event.buttons === undefined) {
    return true;
  }

  if (event.button === 0 && event.buttons === undefined) {
    return true;
  }

  if (event.button === 0 && event.buttons === 0) {
    return true;
  }

  if (event.button !== 0 || event.buttons !== 1) {
    return false;
  }

  return true;
}
```

注：参考来源[` isSingleLeftClick `](https://github.com/videojs/video.js/blob/master/src/js/utils/dom.js#L761)


## class

### hasClass

判断某元素` el ` 是否有类` clsName `。如果有返回true，否则返回false。

```
function hasClass(el, clsName) {
  clsName = clsName.trim()
  if (el.classList) {
    return el.classList.contains(clsName);
  }
  return (new RegExp('(^|\\s)' + clsName + '($|\\s)')).test(el.className);
}
```

## range

### isBottom

判断是否` 到达页面底部 `。如果是返回true，否则返回false。

```
funciton isBottom() {
  return document.documentElement.clientHeight + window.scrollY >=
  (document.documentElement.scrollHeight || document.documentElement.clientHeight);
}
```

### isTop

判断是否` 到达页面顶部 `。如果是返回true，否则返回false。

```
funciton isTop() {
  return (window.scrollY || document.documentElement.scrollTop) <= 0;
}
```

### elementIsVisibleInViewport

判断元素` el `是否出现在` 浏览器窗口 `。如果是返回true，否则返回false。

```
const elementIsVisibleInViewport = (el, partiallyVisible = false) => {
  const { top, left, bottom, right } = el.getBoundingClientRect();
  const { innerHeight, innerWidth } = window;
  return partiallyVisible
    ? ((top > 0 && top < innerHeight) || (bottom > 0 && bottom < innerHeight)) &&
        ((left > 0 && left < innerWidth) || (right > 0 && right < innerWidth))
    : top >= 0 && left >= 0 && bottom <= innerHeight && right <= innerWidth;
};
```

注：参考来源[` 30-second-code elementIsVisibleInViewport `](https://github.com/30-seconds/30-seconds-of-code#elementisvisibleinviewport-)
