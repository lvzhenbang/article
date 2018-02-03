
## Date

### getDaysDiffBetweenDates

Returns the difference (in days) between two dates.

Calculate the difference (in days) between two `Date` objects.

```js
const getDaysDiffBetweenDates = (dateInitial, dateFinal) => (dateFinal - dateInitial) / (1000 * 3600 * 24);
// getDaysDiffBetweenDates(new Date("2017-12-13"), new Date("2017-12-22")) -> 9
```


### JSONToDate

Converts a JSON object to a date.

Use `Date()`, to convert dates in JSON format to readable format (`dd/mm/yyyy`).

```js
const JSONToDate = arr => {
  const dt = new Date(parseInt(arr.toString().substr(6)));
  return `${ dt.getDate() }/${ dt.getMonth() + 1 }/${ dt.getFullYear() }`
};
// JSONToDate(/Date(1489525200000)/) -> "14/3/2017"
```


### toEnglishDate

Converts a date from American format to English format.

Use `Date.toISOString()`, `split('T')` and `replace()` to convert a date from American format to the English format.
Throws an error if the passed time cannot be converted to a date.

```js
const toEnglishDate  = (time) =>
  {try{return new Date(time).toISOString().split('T')[0].replace(/-/g, '/')}catch(e){return}};
// toEnglishDate('09/21/2010') -> '21/09/2010'
```

