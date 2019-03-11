## 其他类型到布尔类型的转换

建议使用的方法是`Boolean(undefined)`。

```
Undefined：false
Null：false
Number: 如果是 +0, −0, 或者 NaN，侧位false；其它为true
String: 如果是空字符串，如：''，则为：false；否则，为true
Object:	true
```