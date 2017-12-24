## String

### anagrams

Generates all anagrams of a string (contains duplicates).

Use recursion.
For each letter in the given string, create all the partial anagrams for the rest of its letters.
Use `Array.map()` to combine the letter with each partial anagram, then `Array.reduce()` to combine all anagrams in one array.
Base cases are for string `length` equal to `2` or `1`.

```js
const anagrams = str => {
  if (str.length <= 2) return str.length === 2 ? [str, str[1] + str[0]] : [str];
  return str.split('').reduce((acc, letter, i) =>
    acc.concat(anagrams(str.slice(0, i) + str.slice(i + 1)).map(val => letter + val)), []);
};
// anagrams('abc') -> ['abc','acb','bac','bca','cab','cba']
```


### Capitalize

Capitalizes the first letter of a string.

Use destructuring and `toUpperCase()` to capitalize first letter, `...rest` to get array of characters after first letter and then `Array.join('')` to make it a string again.
Omit the `lowerRest` parameter to keep the rest of the string intact, or set it to `true` to convert to lowercase.

```js
const capitalize = ([first,...rest], lowerRest = false) =>
  first.toUpperCase() + (lowerRest ? rest.join('').toLowerCase() : rest.join(''));
// capitalize('myName') -> 'MyName'
// capitalize('myName', true) -> 'Myname'
```


### capitalizeEveryWord

Capitalizes the first letter of every word in a string.

Use `replace()` to match the first character of each word and `toUpperCase()` to capitalize it.

```js
const capitalizeEveryWord = str => str.replace(/\b[a-z]/g, char => char.toUpperCase());
// capitalizeEveryWord('hello world!') -> 'Hello World!'
```


### countVowels

Retuns `number` of vowels in provided string.

Use a regular expression to count the number of vowels `(A, E, I, O, U)` in a `string`.

```js
const countVowels = str => (str.match(/[aeiou]/ig) || []).length;
// countVowels('foobar') -> 3
// countVowels('gym') -> 0
```


### escapeRegExp

Escapes a string to use in a regular expression.

Use `replace()` to escape special characters.

```js
const escapeRegExp = str => str.replace(/[.*+?^${}()|[\]\\]/g, '\\$&');
// escapeRegExp('(test)') -> \\(test\\)
```


### fromCamelCase

Converts a string from camelcase.

Use `replace()` to remove underscores, hyphens, and spaces and convert words to camelcase.
Omit the second argument to use a default separator of `_`.

```js
const fromCamelCase = (str, separator = '_') =>
  str.replace(/([a-z\d])([A-Z])/g, '$1' + separator + '$2')
    .replace(/([A-Z]+)([A-Z][a-z\d]+)/g, '$1' + separator + '$2').toLowerCase();
// fromCamelCase('someDatabaseFieldName', ' ') -> 'some database field name'
// fromCamelCase('someLabelThatNeedsToBeCamelized', '-') -> 'some-label-that-needs-to-be-camelized'
// fromCamelCase('someJavascriptProperty', '_') -> 'some_javascript_property'
```


### reverseString

Reverses a string.

Use `split('')` and `Array.reverse()` to reverse the order of the characters in the string.
Combine characters to get a string using `join('')`.

```js
const reverseString = str => str.split('').reverse().join('');
// reverseString('foobar') -> 'raboof'
```


### sortCharactersInString

Alphabetically sorts the characters in a string.

Split the string using `split('')`, `Array.sort()` utilizing `localeCompare()`, recombine using `join('')`.

```js
const sortCharactersInString = str =>
  str.split('').sort((a, b) => a.localeCompare(b)).join('');
// sortCharactersInString('cabbage') -> 'aabbceg'
```


### toCamelCase

Converts a string to camelcase.

Use `replace()` to remove underscores, hyphens, and spaces and convert words to camelcase.

```js
const toCamelCase = str =>
  str.replace(/^([A-Z])|[\s-_]+(\w)/g, (match, p1, p2, offset) =>  p2 ? p2.toUpperCase() : p1.toLowerCase());
// toCamelCase("some_database_field_name") -> 'someDatabaseFieldName'
// toCamelCase("Some label that needs to be camelized") -> 'someLabelThatNeedsToBeCamelized'
// toCamelCase("some-javascript-property") -> 'someJavascriptProperty'
// toCamelCase("some-mixed_string with spaces_underscores-and-hyphens") -> 'someMixedStringWithSpacesUnderscoresAndHyphens'
```


### truncateString

Truncates a string up to a specified length.

Determine if the string's `length` is greater than `num`.
Return the string truncated to the desired length, with `...` appended to the end or the original string.

```js
const truncateString = (str, num) =>
  str.length > num ? str.slice(0, num > 3 ? num - 3 : num) + '...' : str;
// truncateString('boomerang', 7) -> 'boom...'
```


### words

Converts a given string into an array of words.

Use `String.split()` with a supplied pattern (defaults to non-alpha as a regex) to convert to an array of strings. Use `Array.filter()` to remove any empty strings.
Omit the second argument to use the default regex.

```js
const words = (str, pattern = /[^a-zA-Z-]+/) => str.split(pattern).filter(Boolean);
// words("I love javaScript!!") -> ["I", "love", "javaScript"]
// words("python, javaScript & coffee") -> ["python", "javaScript", "coffee"]
```
