
## Nodejs

### JSONToFile

将一个json对象保存到文件中.

用 `fs.writeFile()`, 模板字符串 和 `JSON.stringify()` 将一个 `json` object 写到一个 `.json` 文件中.

```js
const fs = require('fs');
const JSONToFile = (obj, filename) => fs.writeFile(`${filename}.json`, JSON.stringify(obj, null, 2))
// JSONToFile({test: "is passed"}, 'testJsonFile') -> 写这个对象到 'testJsonFile.json'
```


### readFileLines

读取指定文件中的内容，按行插入数组中.

用 `node` 的 `fs` 模块中的 `readFileSync` 方法读取文件后创建一个 `Buffer`.
用 `toString(encoding)` 将 `Buffer` 转换为字符串.
用 `split` 以 `\n` 为分隔符来创建一个行文件内容数组.

  ```js
const fs = require('fs');
const readFileLines = filename => fs.readFileSync(filename).toString('UTF8').split('\n');
/*
  contents of test.txt :
    line1
    line2
    line3
    ___________________________
  let arr = readFileLines('test.txt')
  console.log(arr) // -> ['line1', 'line2', 'line3']
 */
```
