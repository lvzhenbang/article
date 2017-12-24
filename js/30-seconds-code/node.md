
## Node

### JSONToFile

Writes a JSON object to a file.

Use `fs.writeFile()`, template literals and `JSON.stringify()` to write a `json` object to a `.json` file.

```js
const fs = require('fs');
const JSONToFile = (obj, filename) => fs.writeFile(`${filename}.json`, JSON.stringify(obj, null, 2))
// JSONToFile({test: "is passed"}, 'testJsonFile') -> writes the object to 'testJsonFile.json'
```


### readFileLines

Returns an array of lines from the specified file.

Use `readFileSync` function in `fs` node package to create a `Buffer` from a file.
convert buffer to string using `toString(encoding)` function.
creating an array from contents of file by `split`ing file content line by line (each `\n`).

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
