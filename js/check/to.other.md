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

注：` isString `可参考[` js.type isString `](https://github.com/lvzhenbang/article/blob/master/js/check/js.type.md#isstring)

## toArray

判断给定的值是否可转化为` 数组 `。如果可以，返回转化后的数组；否则返回原值。

```
function toArray (val) {
  return isArrayLike(val) ? Array.prototype.slice.call(val || [], 0) : val;
}
```

注：` isArrayLike `可参考[` js.type isArrayLike `](https://github.com/lvzhenbang/article/blob/master/js/check/js.type.md#isarraylike)
