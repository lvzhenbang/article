
## Math

### arrayAverage

Returns the average of an array of numbers.

Use `Array.reduce()` to add each value to an accumulator, initialized with a value of `0`, divide by the `length` of the array.

```js
const arrayAverage = arr => arr.reduce((acc, val) => acc + val, 0) / arr.length;
// arrayAverage([1,2,3]) -> 2
```


### arraySum

Returns the sum of an array of numbers.

Use `Array.reduce()` to add each value to an accumulator, initialized with a value of `0`.

```js
const arraySum = arr => arr.reduce((acc, val) => acc + val, 0);
// arraySum([1,2,3,4]) -> 10
```


### clampNumber

Clamps `num` within the inclusive `lower` and `upper` bounds.

If `lower` is greater than `upper`, swap them. 
If `num` falls within the range, return `num`. 
Otherwise, return the nearest number in the range.

```js
const clampNumber = (num, lower, upper) => {
  if(lower > upper) upper = [lower, lower = upper][0];
  return (num>=lower && num<=upper) ? num : ((num < lower) ? lower : upper) 
}
// clampNumber(2, 3, 5) -> 3
// clampNumber(1, -1, -5) -> -1
// clampNumber(3, 2, 4) -> 3
```


### collatz

Applies the Collatz algorithm.

If `n` is even, return `n/2`. Otherwise, return `3n+1`.

```js
const collatz = n => (n % 2 == 0) ? (n / 2) : (3 * n + 1);
// collatz(8) --> 4
// collatz(5) --> 16
```


### digitize

Converts a number to an array of digits.

Convert the number to a string, using spread operators in ES6(`[...string]`) build an array.
Use `Array.map()` and `parseInt()` to transform each value to an integer.

```js
const digitize = n => [...''+n].map(i => parseInt(i));
// digitize(2334) -> [2, 3, 3, 4]
```


### distance

Returns the distance between two points.

Use `Math.hypot()` to calculate the Euclidean distance between two points.

```js
const distance = (x0, y0, x1, y1) => Math.hypot(x1 - x0, y1 - y0);
// distance(1,1, 2,3) -> 2.23606797749979
```


### factorial

Calculates the factorial of a number.

Use recursion.
If `n` is less than or equal to `1`, return `1`.
Otherwise, return the product of `n` and the factorial of `n - 1`.
Throws an exception if `n` is a negative number.

```js
const factorial = n =>
  n < 0 ? (() => { throw new TypeError('Negative numbers are not allowed!') })()
  : n <= 1 ? 1 : n * factorial(n - 1);
// factorial(6) -> 720
```


### fibonacci

Generates an array, containing the Fibonacci sequence, up until the nth term.

Create an empty array of the specific length, initializing the first two values (`0` and `1`).
Use `Array.reduce()` to add values into the array, using the sum of the last two values, except for the first two.

```js
const fibonacci = n =>
  Array.from({ length: n}).reduce((acc, val, i) => acc.concat(i > 1 ? acc[i - 1] + acc[i - 2] : i), []);
// fibonacci(5) -> [0,1,1,2,3]
```


### fibonacciCountUntilNum

Returns the number of fibonnacci numbers up to `num`(`0` and `num` inclusive).

Use a mathematical formula to calculate the number of fibonacci numbers until `num`.

```js
const fibonacciCountUntilNum = num =>
  Math.ceil(Math.log(num * Math.sqrt(5) + 1/2) / Math.log((Math.sqrt(5)+1)/2));
// fibonacciCountUntilNum(10) -> 7
```


### fibonacciUntilNum

Generates an array, containing the Fibonacci sequence, up until the nth term.

Create an empty array of the specific length, initializing the first two values (`0` and `1`).
Use `Array.reduce()` to add values into the array, using the sum of the last two values, except for the first two.
Uses a mathematical formula to calculate the length of the array required.

```js
const fibonacciUntilNum = num => {
  let n = Math.ceil(Math.log(num * Math.sqrt(5) + 1/2) / Math.log((Math.sqrt(5)+1)/2));
  return Array.from({ length: n}).reduce((acc, val, i) => acc.concat(i > 1 ? acc[i - 1] + acc[i - 2] : i), []);
}
// fibonacciUntilNum(15) -> [0,1,1,2,3,5,8,13]
```


### gcd

Calculates the greatest common divisor between two numbers.

Use recursion.
Base case is when `y` equals `0`. In this case, return `x`.
Otherwise, return the GCD of `y` and the remainder of the division `x/y`.

```js
const gcd = (x, y) => !y ? x : gcd(y, x % y);
// gcd (8, 36) -> 4
```


### hammingDistance

Calculates the Hamming distance between two values.

Use XOR operator (`^`) to find the bit difference between the two numbers, convert to a binary string using `toString(2)`.
Count and return the number of `1`s in the string, using `match(/1/g)`.

```js
const hammingDistance = (num1, num2) =>
  ((num1 ^ num2).toString(2).match(/1/g) || '').length;
// hammingDistance(2,3) -> 1
```


### inRange

Checks if the given number falls within the given range. 

Use arithmetic comparison to check if the given number is in the specified range.
If the second parameter, `end`, is not specified, the range is considered to be from `0` to `start`.

```js
const inRange = (n, start, end=null) => {
  if(end && start > end) end = [start, start=end][0];
  return (end == null) ? (n>=0 && n<start) : (n>=start && n<end);
}
// inRange(3, 2, 5) -> true
// inRange(3, 4) -> true
// inRange(2, 3, 5) -> false
// inrange(3, 2) -> false
```


### isArmstrongNumber

Checks if the given number is an Armstrong number or not.

Convert the given number into an array of digits. Use `Math.pow()` to get the appropriate power for each digit and sum them up. If the sum is equal to the number itself, return `true` otherwise `false`.

```js
const isArmstrongNumber = digits => 
  ( arr => arr.reduce( ( a, d ) => a + Math.pow( parseInt( d ), arr.length ), 0 ) == digits ? true : false )( ( digits+'' ).split( '' ) );
// isArmstrongNumber(1634) -> true
// isArmstrongNumber(371) -> true
// isArmstrongNumber(56) -> false
```


### isDivisible

Checks if the first numeric argument is divisible by the second one.

Use the modulo operator (`%`) to check if the remainder is equal to `0`.

```js
const isDivisible = (dividend, divisor) => dividend % divisor === 0;
// isDivisible(6,3) -> true
```


### isEven

Returns `true` if the given number is even, `false` otherwise.

Checks whether a number is odd or even using the modulo (`%`) operator.
Returns `true` if the number is even, `false` if the number is odd.

```js
const isEven = num => num % 2 === 0;
// isEven(3) -> false
```


### isPrime

Checks if the provided integer is a prime number.

Check numbers from `2` to the square root of the given number. 
Return `false` if any of them divides the given number, else return `true`, unless the number is less than `2`.

```js
const isPrime = num => {
  const boundary = Math.floor(Math.sqrt(num));
  for (var i = 2; i * i <= boundary; i++) if (num % i == 0) return false;
  return num >= 2;
};
// isPrime(11) -> true
// isPrime(12) -> false
```


### lcm

Returns the least common multiple of two numbers.

Use the greatest common divisor (GCD) formula and `Math.abs()` to determine the least common multiple.
The GCD formula uses recursion.

```js
const lcm = (x,y) => {
  const gcd = (x, y) => !y ? x : gcd(y, x % y);
  return Math.abs(x*y)/(gcd(x,y));
};
// lcm(12,7) -> 84
```


### median

Returns the median of an array of numbers.

Find the middle of the array, use `Array.sort()` to sort the values.
Return the number at the midpoint if `length` is odd, otherwise the average of the two middle numbers.

```js
const median = arr => {
  const mid = Math.floor(arr.length / 2), nums = [...arr].sort((a, b) => a - b);
  return arr.length % 2 !== 0 ? nums[mid] : (nums[mid - 1] + nums[mid]) / 2;
};
// median([5,6,50,1,-5]) -> 5
// median([0,10,-2,7]) -> 3.5
```


### palindrome

Returns `true` if the given string is a palindrome, `false` otherwise.

Convert string `toLowerCase()` and use `replace()` to remove non-alphanumeric characters from it.
Then, `split('')` into individual characters, `reverse()`, `join('')` and compare to the original, unreversed string, after converting it `tolowerCase()`.

```js
const palindrome = str => {
  const s = str.toLowerCase().replace(/[\W_]/g,'');
  return s === s.split('').reverse().join('');
}
// palindrome('taco cat') -> true
 ```


### percentile

Uses the percentile formula to calculate how many numbers in the given array are less or equal to the given value.

Use `Array.reduce()` to calculate how many numbers are below the value and how many are the same value and apply the percentile formula.

```js
const percentile = (arr, val) =>
  100 * arr.reduce((acc,v) => acc + (v < val ? 1 : 0) + (v === val ? 0.5 : 0), 0) / arr.length;
// percentile([1,2,3,4,5,6,7,8,9,10], 6) -> 55
 ```


### powerset

Returns the powerset of a given array of numbers.

Use `Array.reduce()` combined with `Array.map()` to iterate over elements and combine into an array containing all combinations.

```js
const powerset = arr =>
  arr.reduce((a, v) => a.concat(a.map(r => [v].concat(r))), [[]]);
// powerset([1,2]) -> [[], [1], [2], [2,1]]
```


### primes 

Generates primes up to a given number, using the Sieve of Eratosthenes.

Generate an array from `2` to the given number. Use `Array.filter()` to filter out the values divisible by any number from `2` to the square root of the provided number.

```js
const primes = num => {
  let arr =  Array.from({length:num-1}).map((x,i)=> i+2), 
    sqroot  = Math.floor(Math.sqrt(num)),
    numsTillSqroot  = Array.from({length:sqroot-1}).map((x,i)=> i+2);
  numsTillSqroot.forEach(x => arr = arr.filter(y => ((y%x)!==0)||(y==x)));
  return arr; 
}
// primes(10) -> [2,3,5,7] 
```


### randomIntegerInRange

Returns a random integer in the specified range.

Use `Math.random()` to generate a random number and map it to the desired range, using `Math.floor()` to make it an integer.

```js
const randomIntegerInRange = (min, max) => Math.floor(Math.random() * (max - min + 1)) + min;
// randomIntegerInRange(0, 5) -> 2
```


### randomNumberInRange

Returns a random number in the specified range.

Use `Math.random()` to generate a random value, map it to the desired range using multiplication.

```js
const randomNumberInRange = (min, max) => Math.random() * (max - min) + min;
// randomNumberInRange(2,10) -> 6.0211363285087005
```


### round

Rounds a number to a specified amount of digits.

Use `Math.round()` and template literals to round the number to the specified number of digits.
Omit the second argument, `decimals` to round to an integer.

```js
const round = (n, decimals=0) => Number(`${Math.round(`${n}e${decimals}`)}e-${decimals}`);
// round(1.005, 2) -> 1.01
```


### standardDeviation

Returns the standard deviation of an array of numbers.

Use `Array.reduce()` to calculate the mean, variance and the sum of the variance of the values, the variance of the values, then
determine the standard deviation.
You can omit the second argument to get the sample standard deviation or set it to `true` to get the population standard deviation.

```js
const standardDeviation = (arr, usePopulation = false) => {
  const mean = arr.reduce((acc, val) => acc + val, 0) / arr.length;
  return Math.sqrt(
    arr.reduce((acc, val) => acc.concat(Math.pow(val - mean, 2)), [])
       .reduce((acc, val) => acc + val, 0) / (arr.length - (usePopulation ? 0 : 1))
  );
};
// standardDeviation([10,2,38,23,38,23,21]) -> 13.284434142114991 (sample)
// standardDeviation([10,2,38,23,38,23,21], true) -> 12.29899614287479 (population)
```
