# 其他类型转换到对象类型

在js中，` 简单对象 `和` 函数 `都可以称之为对象。

常用的其它类型转换为对象如下：

```
// string -> object
JSON.parse('{"a":1,"b":2}') // {a: 1, b: 2}
JSON.parse('[1, 2, 3]') // [1, 2, 3]
// ObjectLike -> object


```