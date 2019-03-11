# 其他类型转换到字符串类型

## 常用方法

1. 不要使用` new String() `，它返回的是一个对象

```
var obj = { 'a': 1 }

console.log(new String(obj.a)) // {"1"}
```

2. 如果喜欢使用` .toString() `方法的，需要注意这个方法是定义类型内置的方法，有些类型没有带，比如：` null `

```
var a = null

console.log(a.toString())

// Uncaught TypeError: Cannot read property 'toString' of null
```

3. 当然还有的同学比较喜欢使用` obj.a + '' `，这种形式同` .toString() `，它默认调用了` .valueOf() ` 方法

4. 这是一个推荐使用的方法，使用` String(obj.a) `

```
var a = { 'a': 1 }

String(obj.a) // '1'
```

5. 但要注意一个特殊的情况，` String(obj) ` 返回` "[object Object]" `

```
var a = { 'a': 1 }

String(obj) // "[object Object]"
```

这里可以使用` JSON.stringify(obj) `，它返回` "{"a":1}" `。


## demo

```
// null
String(null) // "null"
// undefined
String(undefined) // "undefined"
// 布尔
String(true) // "true"
// 数字
String(1) // "1"
String([1, 2, 3]) // "[1, 2, 3]"
// 对象
var obj = { 'a': 1 }
String(obj.a) // "1"
JSON.stringify(obj) // "{ "a": 1 }"
// 函数
funciton hello() {
  console.log('hello world')
}
String(hello)
/**
  *  "function hello() {
  *    console.log('hello world')
  *  }"
*/
```