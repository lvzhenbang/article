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


## URL

### isHttps

判断当前页面是否使用的是`HTTPS`协议。如果是返回true，否则返回false。

```
function isHttps () {
  return window.location.href.test(/^https/);
}
```

### isObjectURL

判断某个值` val `是否是[` ObjectURL `](https://developer.mozilla.org/zh-CN/docs/Web/API/URL/createObjectURL)。如果是返回true，否则返回false。

```
function isObjectURL (val) {
  return val.indexOf('blob:') === 0;
}
```

### isURLSearchParams

判断给定的值是否为` URLSearchParams `。如果是返回true，否则返回false。

```
function isURLSearchParams(val) {
  return typeof URLSearchParams !== 'undefined' && val instanceof URLSearchParams;
}
```

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

## Device

### isTouchDevice

判断是否是` 触屏设备 `。如果是返回true，否则返回false。

```
function isTouchDevice () {
  // works on most browsers
  if ('ontouchstart' in window) {
    return true
  }

  // works on IE10/11 and Surface
  return !!navigator.maxTouchPoints
}
```

### isDragDropSupported

判断是否支持` 元素拖拽 `。如果是返回true，否则返回false。

```
function isDragDropSupported () {
  var div = document.createElement('div')

  if (!('draggable' in div) || !('ondragstart' in div && 'ondrop' in div)) {
    return false
  }

  if (!('FormData' in window)) {
    return false
  }

  if (!('FileReader' in window)) {
    return false
  }

  return true
}
```

## file

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

### isPreviewFile

判断` FileType ` 是否可以在 ` 浏览器中预览 `。如果是返回true，否则返回false。

```
function isPreviewFile (val) {
  if (!val) return false
  const fileType = val.split('/')[1]
  if (/^(jpe?g|gif|png|svg|svg\+xml|bmp)$/.test(fileType)) {
    return true
  }
  return false
}
```

注：参考[` uppy isPreviewSupported`](https://github.com/transloadit/uppy/blob/master/packages/%40uppy/utils/src/isPreviewSupported.js)

## date

### isDate

判断给定的值` val `是否为` date `。如果是返回true，否则返回false。

```
function isDate(val) {
  return _toString.call(val) === '[object Date]';
}
```

注：一般不使用` isDate() `，而是使用下面的` isValidDate(val) `。

### isValidDate

判断给定的值` val `是否为有效的` date `。如果是返回true，否则返回false。

```
funciton isValidDate(val) {
  return isDate(val) && !isNaN(val.getTime())
}
```

### isLeapYear

判断给定的` 年份 `值` val `是否为` 闰年 `。如果是返回true，否则返回false。

```
function isLeapYaer(val) {
  var n = parseInt(val);
  return !isNaN(n) && n % 400 === 0 || (n % 4 === 0 && n % 100 !== 0);
}
```

### isSameYear

判断两个给定的` date `对象` val `和`val2`是否为` 同一年 `。如果是返回true，否则返回false。

```
function isSameYear(val, val2) {
  return val.getFullYear() === val2.getFullYear();
}
```

### isSameMonth

判断两个给定的` date `对象` val `和`val2`是否为` 同一年 `。如果是返回true，否则返回false。

```
function isSameMonth(val, val2) {
  return isSameYear(val, val2) && val.getMonth() === val2.getMonth();
}
```

### isSameWeek

判断两个给定的` date `对象` val `和`val2`是否为` 同一周 `。如果是返回true，否则返回false。

```
function isSameMonth(val, val2) {
  return startOfWork(val).getTime === startOfWork(val2).getTime;
}
```

### isSameDay

判断两个给定的` date `对象` val `和`val2`是否为` 同一年 `。如果是返回true，否则返回false。

```
function isSameDay(val, val2) {
  return val.getTime === val2.getTime;
}
```

注：[` Date.getTime() `](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Date/getTime) 返回一个从` Jan 1, 1970, 00:00:00.000 GMT `的毫秒数值。

### isWeekend

判断给定的` date `值` val `是否为` 周末 `。如果是返回true，否则返回false。

```
function isWeekend(val) {
  var day = date.getDay();
  return day === 0 || day === 6;
}
```
