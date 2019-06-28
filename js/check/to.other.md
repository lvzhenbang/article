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

## toObject

判断给定的值是否可转化为` 对象 `。如果可以，返回转化后的数字；否则返回原值。

```
/* string -> object
JSON.parse('{"a":1,"b":2}') // {a: 1, b: 2}
JSON.parse('[1, 2, 3]') // [1, 2, 3]
*/
function toObject(val) {
  return JSON.parse(val)
}
```