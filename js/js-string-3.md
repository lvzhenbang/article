## 常用的字符串方法

在ES6及以后增加的方法

### String.prototype.padEnd()/String.prototype.padStart()

描述：它被用来填充字符串。它们允许在原始字符串的开始和结尾处添加空字符串或这其它形式的字符串。

用法：

'someString'.padStart(targetLength[, padString])
'someString'.padEnd(字符串的总长度[,填充的字符串])

```
var str = 'str'

str.padEnd(10, '--') // "str-------"
```