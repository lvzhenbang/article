# 其他类型转换到数字类型

1. 不要使用` new Number() `，它返回一个对象

```
new Number('1') // {1}
```

2. 不要使用` + '1' `这种方式

3. 不要使用` '1' >> 0 `这种方式，这种方式虽然快，但是，它有范围限制，如果你的电脑是32位的，那么会出现下面这种情况

```
2147483647 >> 0; // => 2147483647
2147483648 >> 0; // => -2147483648
2147483649 >> 0; // => -2147483647
```

注：这在做金融性的项目时，会出现大的问题。

4. 可以考虑使用` parseInt('1', 10) `，也可以考虑使用 `Number('1')`

## 其它类型强制转换到数字类型的结果

```
// null
console.log(Number(null)) // 0
// undefined
console.log(Number(undefined)) // NaN
// 字符串
console.log(Number('abc')) // NaN
console.log(Number('1')) // 1
// 布尔
console.log(Number(true)) // 1
// 数组
console.log(Number([1, 2, 3])) // NaN
// 对象
console.log(Number({ 'a': 1 })) // NaN
// 函数
console.log(Number(()=> console.log(1))) // NaN
```

注：`parseInt()`和`Number`类似，不同的是它有两个参数，第一个是需要转换的值，第二个是指定的[`进制`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/parseInt#%E5%8F%82%E6%95%B0)类型。

通过上面的结果，可以发现，其他类类型的数据要转换到数字，大多数没有实际意义。最有意义的字符串到数字的转换。`form`表单`input`中的字符串类型的值需要转换为数字。
