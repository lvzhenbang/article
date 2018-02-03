## String

### anagrams

计算一个字符串中字符的所有排序情况.

使用递归.
遍历字符串中的每个字符, 计算剩余字符串的所有顺序.
用 `Array.map()` 区合并该字符和剩余字符串的每种顺序, 然后用 `Array.reduce()` 将该字符串的所有顺序合并到一个数组中.
当字符串的 `length` 等于 `2` 或者 `1` 时，是两个基例.

```js
const anagrams = str => {
  if (str.length <= 2) return str.length === 2 ? [str, str[1] + str[0]] : [str];
  return str.split('').reduce((acc, letter, i) =>
    acc.concat(anagrams(str.slice(0, i) + str.slice(i + 1)).map(val => letter + val)), []);
};
// anagrams('abc') -> ['abc','acb','bac','bca','cab','cba']
```


### Capitalize

字符串的首字母大写.

用解构和 `toUpperCase()` 将首字母大写, 用 `...rest` 去获得除首字母外的字符串，然后用 `Array.join('')` 将所得的数组合并为字符串.
如果 `lowerRest`参数为 `true`, 将转变剩余字符串为小写输出, 否则.原样输出.

```js
const capitalize = ([first,...rest], lowerRest = false) =>
  first.toUpperCase() + (lowerRest ? rest.join('').toLowerCase() : rest.join(''));
// capitalize('myName') -> 'MyName'
// capitalize('myName', true) -> 'Myname'
```


### capitalizeEveryWord

把字符串的每个单词首字母大写.

用 `replace()` 去匹配每个单词的首字母 然后用 `toUpperCase()` 将该字母大写.

```js
const capitalizeEveryWord = str => str.replace(/\b[a-z]/g, char => char.toUpperCase());
// capitalizeEveryWord('hello world!') -> 'Hello World!'
```


### countVowels

计算字串中原因字母个数.

用一个正则表达是计算字符串中的元音字母 `(A, E, I, O, U)` 的个数.

```js
const countVowels = str => (str.match(/[aeiou]/ig) || []).length;
// countVowels('foobar') -> 3
// countVowels('gym') -> 0
```


### escapeRegExp

转义一个字符串为一个可使用的正则表达式.

用 `replace()` 转义特殊的字符.

```js
const escapeRegExp = str => str.replace(/[.*+?^${}()|[\]\\]/g, '\\$&');
// escapeRegExp('(test)') -> \\(test\\)
```


### fromCamelCase

转换驼峰字符串.

用 `replace()` 以下划线，连字符和空格为分隔符转换驼峰形式的字符串，首先需要去除字符串中的分割字符，然后，用分隔符拼接字符串.
第二个参数默认为 `_`.

```js
const fromCamelCase = (str, separator = '_') =>
  str.replace(/([a-z\d])([A-Z])/g, '$1' + separator + '$2')
    .replace(/([A-Z]+)([A-Z][a-z\d]+)/g, '$1' + separator + '$2').toLowerCase();
// fromCamelCase('someDatabaseFieldName', ' ') -> 'some database field name'
// fromCamelCase('someLabelThatNeedsToBeCamelized', '-') -> 'some-label-that-needs-to-be-camelized'
// fromCamelCase('someJavascriptProperty', '_') -> 'some_javascript_property'
```


### toCamelCase

字符串转化为驼峰形式的字符串.

用 `replace()` 去移除下划线，连接符，空格，然后将字符串转化为驼峰形式输出.

```js
const toCamelCase = str =>
  str.replace(/^([A-Z])|[\s-_]+(\w)/g, (match, p1, p2, offset) =>  p2 ? p2.toUpperCase() : p1.toLowerCase());
// toCamelCase("some_database_field_name") -> 'someDatabaseFieldName'
// toCamelCase("Some label that needs to be camelized") -> 'someLabelThatNeedsToBeCamelized'
// toCamelCase("some-javascript-property") -> 'someJavascriptProperty'
// toCamelCase("some-mixed_string with spaces_underscores-and-hyphens") -> 'someMixedStringWithSpacesUnderscoresAndHyphens'
```


### reverseString

反转字符串.

用 `split('')` 和 `Array.reverse()` 去反转字符数组中元素的顺序.
用 `join('')` 去拼字符数组为字符串.

```js
const reverseString = str => str.split('').reverse().join('');
// reverseString('foobar') -> 'raboof'
```


### sortCharactersInString

按字符顺序排列字符串中字符的顺序.

用 `split('')` 将字符串转化为数组, 然后用 `Array.sort()` 依据自定义排序规则 `localeCompare()` 位字符数组排序, 最后用 `join('')` 重新拼接字符数组为字符串.

```js
const sortCharactersInString = str =>
  str.split('').sort((a, b) => a.localeCompare(b)).join('');
// sortCharactersInString('cabbage') -> 'aabbceg'
```


### truncateString


保留指定长度的字符串.

需要判断字符串的 `length` 是否大于 `num`.
返回你希望的截断后的字符串, 尾部有 `...`.

```js
const truncateString = (str, num) =>
  str.length > num ? str.slice(0, num > 3 ? num - 3 : num) + '...' : str;
// truncateString('boomerang', 7) -> 'boom...'
```


### words

将字符串转化为以单词为元素的字符串数组,去除所有非单词元素.

用 `String.split()` 以 `pattern` 正则表达式为分割规则来分割字符串为字符串数组. 用 `Array.filter()` 移除字符串中的空字符.
默认 `pattern` 为/[^a-zA-Z-]+/.

```js
const words = (str, pattern = /[^a-zA-Z-]+/) => str.split(pattern).filter(Boolean);
// words("I love javaScript!!") -> ["I", "love", "javaScript"]
// words("python, javaScript & coffee") -> ["python", "javaScript", "coffee"]
```
