# 将给定的值转换为指定类型


## toNumber

判断给定的值是否可转化为` 数字 `。如果可以，返回转化后的数字；否则返回原值。

```
function toNumber(val) {
  var n = parseFloat(val)
  return isNaN(n) ? val : n
}
```

## toString

如果给定的值是` 数组 ` 或者 `普通对象`，则使用`JSON.stringify(val)`；否则使用` String(val) `。

```
function toString (val) {
  return Array.isArray(val) || (isPlainObject(val) && val.toString === Object.prototype.toString)
    ? JSON.stringify(val, null, 2)
    : String(val)
}
```

注：参考[` vue toString `](https://github.com/vuejs/vue/blob/dev/src/shared/util.js#L85)

注：` isPlainObject `可参考[` js.other isPlainObject `](https://github.com/lvzhenbang/article/blob/master/js/check/js.type.md#isplainobject)

## toObject

判断给定的值是否可转化为` 对象 `。如果可以，返回转化后的数字；否则返回原值。

```
/* string -> object
JSON.parse('{"a":1,"b":2}') // {a: 1, b: 2}
JSON.parse('[1, 2, 3]') // [1, 2, 3]
*/
function toObject(val) {
  return isString(val) && /^({|\[)(}|\])$/.test(val) ? JSON.parse(val) : val;
}
```

注：` isString `可参考[` js.type isString `](https://github.com/lvzhenbang/article/blob/master/js/check/js.type.md#isString)

## toArray

判断给定的值是否可转化为` 数组 `。如果可以，返回转化后的数组；否则返回原值。

```
function toArray (val) {
  return isArrayLike(val) ? Array.prototype.slice.call(val || [], 0) : val;
}
```

注：` isArrayLike `可参考[` js.type isArrayLike `](https://github.com/lvzhenbang/article/blob/master/js/check/js.type.md#isarraylike)

## dataURItoMimeType

获取` base64 `所指代的` mimeType `。

```
funciton dataURItoMimeType (dataURI, opts) {
  return opts.mimeType || dataURI.split(',')[0].split(':')[1].split(';')[0];
}
```

## dataURItoByetes

转化` base64 `数据为` bytes `。

```
function dataURItoBlob (dataURI, opts) {
  const data = dataURI.split(',')[1];
  const mimeTpe = dataURItoMimeType(dataURI, opts);

  if (mimeType == null) {
    mimeType = 'plain/text';
  }

  const binary = atob(data);
  const array = [];
  for (let i = 0; i < binary.length; i++) {
    array.push(binary.charCodeAt(i));
  }

  let bytes;
  try {
    bytes = new Uint8Array(array);
  } catch (err) {
    return null;
  }
  return bytes;
}
```

## dataURItoBlob

转化` base64 `数据为` blob `。

```
function dataURItoBlob (dataURI, opts) {
  return new Blob([dataURItoByetes(dataURI, opts)], { type: dataURItoMimeType(dataURI, opts) });
}
```

## dataURItoFile

转化` base64 `数据为` file `。

```
function dataURItoFile (dataURI, opts) {
  return new File([dataURItoByetes(dataURI, opts)], opts.name || '', { type: dataURItoMimeType(dataURI, opts) });
}
```

## canvasToBlob

转化` canvas `对象为` blob `。

```
function canvasToBlob (canvas, type, quality) {
  if (canvas.toBlob) {
    return canvas.toBlob(function() {}, type, quality)
  }
  
  return dataURItoBlob(canvas.toDataURL(type, quality), {});
}
```

## toDate

转化` date `对象。

```
function toDate(y, m, d, M, h, s, ms) {
  retunr new Date(y, m, d, M, h, s, ms)
}
```

注：`isDate(val)`参考[` js.type isDate `](https://github.com/lvzhenbang/article/blob/master/js/check/js.type.md#isDate)


## secondsToDate

将` seconds `转化为日期` date `对象，并返回对象。

```
function secondsToDate (seconds) {
  var n = parseInt(seconds);
  return !isNaN(n) && new Date(n);
}
```
注：`isNaN(n)`参考[` js.type isNaN `](https://github.com/lvzhenbang/article/blob/master/js/check/js.type.md#isNaN)