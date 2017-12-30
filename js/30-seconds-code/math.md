
## Math

### 数组求和 (arraySum)

返回数字数组的和.

用 `Array.reduce()` 加每一个value到累加器, 初始值为 `0`.

```js
const arraySum = arr => arr.reduce((acc, val) => acc + val, 0);
// arraySum([1,2,3,4]) -> 10
```


### 数组求平均值( arrayAverage)

返回一个数组的平均值.

用 `Array.reduce()` 加每一个value到累加器, 初始值为 `0`, 除以数组的 `length` .

```js
const arrayAverage = arr => arr.reduce((acc, val) => acc + val, 0) / arr.length;
// arrayAverage([1,2,3]) -> 2
```


### clampNumber

Clamps `num` within the inclusive `lower` and `upper` bounds.

若 `lower` > `upper`, 交换它们. 
若 `num` 在 [lower, upper] 范围内, 返回 `num`. 
否则, 返回离它最近的边界数字.

```js
const clampNumber = (num, lower, upper) => {
  if(lower > upper) upper = [lower, lower = upper][0];
  return (num>=lower && num<=upper) ? num : ((num < lower) ? lower : upper) 
}
// clampNumber(2, 3, 5) -> 3
// clampNumber(1, -1, -5) -> -1
// clampNumber(3, 2, 4) -> 3
```

### 交换两个数字（自定义）

实现两个数字的交换

```js

var a = 2, b = 3;

b = [a, a = b][0]

output: a->3  b->2

ES6：

[a, b] = [b, a]

output: a->3  b->2
```

### collatz

实现 collatz 算法.

如果 `n` 偶数, 返回 `n/2`. 否则, 返回 `3n+1`.

```js
const collatz = n => (n % 2 == 0) ? (n / 2) : (3 * n + 1);
// collatz(8) --> 4
// collatz(5) --> 16
```


### digitize

将一个数字转化为数字数组.

将数字转化为字符串, 用spread 操作符(`[...string]`) 将字符串转变为字符串数组.
用 `Array.map()` 和 `parseInt()` 将字符串组的元素转化为数字.

```js
const digitize = n => [...''+n].map(i => parseInt(i));
// digitize(2334) -> [2, 3, 3, 4]
```


### 距离(distance)

返回两点间的距离.

用 `Math.hypot()` 计算两点间的几何距离.

```js
const distance = (x0, y0, x1, y1) => Math.hypot(x1 - x0, y1 - y0);
// distance(1,1, 2,3) -> 2.23606797749979
```


### 阶乘(factorial)

返回一个属的阶乘.

使用递归.
如果 `n` <= `1`, 返回 `1`.
否则, 返回乘以 `n - 1` 阶乘.
如果 `n` 负数，抛出异常.

```js
const factorial = n =>
  n < 0 ? (() => { throw new TypeError('Negative numbers are not allowed!') })()
  : n <= 1 ? 1 : n * factorial(n - 1);
// factorial(6) -> 720
```


### 斐波那契数列(fibonacci)

生成一个由斐波那契数列数字组成的数组.

创建一个指定长度的空数组, 初始化前两个元素的值为 (`0` , `1`).
用 `Array.reduce()` 向数组中添加数组元素, 从第三个元素开始，每个元素的值为前两个元素的和.

```js
const fibonacci = n =>
  Array.from({ length: n}).reduce((acc, val, i) => acc.concat(i > 1 ? acc[i - 1] + acc[i - 2] : i), []);
// fibonacci(5) -> [0,1,1,2,3]
```


### fibonacciCountUntilNum

返回[`0` , `num` ]范围内Fibonacci数目.

使用数学公式计算到 `num` 的Fibonacci数目.

```js
const fibonacciCountUntilNum = num =>
  Math.ceil(Math.log(num * Math.sqrt(5) + 1/2) / Math.log((Math.sqrt(5)+1)/2));
// fibonacciCountUntilNum(10) -> 7
```


### fibonacciUntilNum

返回一个到 `num` 的Fibonacci数组.

创建一个指定长度的数组, 初始化前两个元素为 (`0` , `1`).
用 `Array.reduce()` 向数组中添加数组元素, 从第三个元素开始，每个元素的值为前两个元素的和.
使用数学公式计算到 `num` 的Fibonacci数目，该数字即为数组的长度.

```js
const fibonacciUntilNum = num => {
  let n = Math.ceil(Math.log(num * Math.sqrt(5) + 1/2) / Math.log((Math.sqrt(5)+1)/2));
  return Array.from({ length: n}).reduce((acc, val, i) => acc.concat(i > 1 ? acc[i - 1] + acc[i - 2] : i), []);
}
// fibonacciUntilNum(15) -> [0,1,1,2,3,5,8,13]
```


### 最大公约数 (gcd)

求两个数间的最大公约数.

用递归.
当 `y` 等于 `0` 返回 `x`.
否则, 返回 gcd(y, x%y).

```js
const gcd = (x, y) => !y ? x : gcd(y, x % y);
// gcd (8, 36) -> 4
```


### 汉明距离 (hammingDistance)

注：它是使用在数据传输差错控制编码里面的，在信息论中用到较多。

计算两个等长字符串的汉明距离。

用异或(XOR)运算符 (`^`) 去找出两个数字对应位不同的数量, 用 `toString(2)` 将字符串转为二进制.
计算并返回字符串中为1的数量, 用 `match(/1/g)` 去匹配.

```js
const hammingDistance = (num1, num2) =>
  ((num1 ^ num2).toString(2).match(/1/g) || '').length;
// hammingDistance(2,3) -> 1
```


### inRange

判断一个数字是否在给定的范围内. 

用算术比较来判断给定的数字是否在指定的方位内.
若第二个参数 `end` 未被指定，这个范围将是 [0 , start] .

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


### isArmstrongNumber （阿姆斯特朗数的判断）

判断一个给定的数字是否是 Armstrong 数字.

所谓Armstrong数，就是n位数的各位数的n次方之和等于该数，如：
  
  153=1^3+5^3+3^3
  1634=1^4+6^4+3^4+4^4

将给定的数字转换为数字数组. 用 `Math.pow()` 为每个数字取合适的指数，然后求和. 如果总和等于数字本身返回 `true`，否则，返回 `false`.

```js
const isArmstrongNumber = digits => 
  ( arr => arr.reduce( ( a, d ) => a + Math.pow( parseInt( d ), arr.length ), 0 ) == digits ? true : false )( ( digits+'' ).split( '' ) );
// isArmstrongNumber(1634) -> true
// isArmstrongNumber(371) -> true
// isArmstrongNumber(56) -> false
```


### isDivisible

判断第一个数字参数能否被第二个数字参数整除.

用模操作符 (`%`) 判断余数是否为 `0`.

```js
const isDivisible = (dividend, divisor) => dividend % divisor === 0;
// isDivisible(6,3) -> true
```


### isEven

如果被给定的数字是偶数，返回  `true` ，否则， `false`.

用模操作符 (`%`) 判断一个数字是奇数还是偶数.
是偶数返回 `true`，是奇数返回 `false` .

```js
const isEven = num => num % 2 === 0;
// isEven(3) -> false
```


### isPrime

判断给定的数字是否为素数.

判断从 `2` 到被给定数的开平方的范围内. 
如果范围内的任何数字整出被给定的数返回 `false`, 否则返回 `true`, 除非给定数字小于 `2`.

```js
const isPrime = num => {
  const boundary = Math.floor(Math.sqrt(num));
  for (var i = 2; i * i <= boundary; i++) if (num % i == 0) return false;
  return num >= 2;
};
// isPrime(11) -> true
// isPrime(12) -> false
```


### 素数 (primes)

生成到 `num` 的素数, 用 `Sieve of Eratosthenes` 算法.

生成一个在 [2, num] 范围内的素数数组. 用 `Array.filter()` 过滤出能被 [2, √num] 间任意整数整除的数字.

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


### 最小公倍数(lcm)

返回两个数的最小公倍数.

用最大公约数 (GCD) 运算式和 `Math.abs()` 来求最小公倍数.
最大公约数 (GCD) 使用递归.

```js
const lcm = (x,y) => {
  const gcd = (x, y) => !y ? x : gcd(y, x % y);
  return Math.abs(x*y)/(gcd(x,y));
};
// lcm(12,7) -> 84
```


### 取中值(median)

返回数字数组中的中间位置数字.

用 `Array.sort()` 去排序数组，找出中间值.
如果数组的 `length` 为奇数，返回中间一位数, 否则返回中间两个数的平局数.

```js
const median = arr => {
  const mid = Math.floor(arr.length / 2), nums = [...arr].sort((a, b) => a - b);
  return arr.length % 2 !== 0 ? nums[mid] : (nums[mid - 1] + nums[mid]) / 2;
};
// median([5,6,50,1,-5]) -> 5
// median([0,10,-2,7]) -> 3.5
```


### 回文字符串(palindrome)

Returns `true` if the given string is a palindrome, `false` otherwise.

将字符串转为小写 `toLowerCase()` 并且用 `replace()` 移除非字母字符.
用 `split('')` 将字符串分割为字符数组, 紧接着用 `reverse()` 反转字符串，用 `join('')` 拼接字符数组为字符串，然后和原字符串比较.

```js
const palindrome = str => {
  const s = str.toLowerCase().replace(/[\W_]/g,'');
  return s === s.split('').reverse().join('');
}
// palindrome('taco cat') -> true
 ```


### 百分位(percentile)

公式为 `p=100*i/n`

用百分位数公式计算给定数组中有多少个数字小于或等于给定值.

用 `Array.reduce()` 去计算给定数组中有多少个数字小于等于给定的值.

```js
const percentile = (arr, val) =>
  100 * arr.reduce((acc,v) => acc + (v < val ? 1 : 0) + (v === val ? 0.5 : 0), 0) / arr.length;
// percentile([1,2,3,4,5,6,7,8,9,10], 6) -> 55
 ```


### 幂集 (powerset)

返回一个指定数组的幂集.

用 `Array.reduce()` 结合 `Array.map()` 迭代所有的元素然后生成一个包含所有元素集合的数组.

```js
const powerset = arr =>
  arr.reduce((a, v) => a.concat(a.map(r => [v].concat(r))), [[]]);
// powerset([1,2]) -> [[], [1], [2], [2,1]]
```

### randomIntegerInRange

返回指定范围的随机整数.

用 `Math.random()` 生成一个随机数，并将其映射到指定范围内, 用 `Math.floor()` 将其转化为一个整数.

```js
const randomIntegerInRange = (min, max) => Math.floor(Math.random() * (max - min + 1)) + min;
// randomIntegerInRange(0, 5) -> 2
```


### randomNumberInRange

返回指定范围内的随机数.

用 `Math.random()` 生成一个随机数, 使用乘法将其映射到所需的范围内.

```js
const randomNumberInRange = (min, max) => Math.random() * (max - min) + min;
// randomNumberInRange(2,10) -> 6.0211363285087005
```


### 四舍五入(round)

数字四舍五入指定位数.

用 `Math.round()` 和模板字面量四舍五入一个数字指定位数.
如果省略第二个参数, `decimals` 默认为0.

```js
const round = (n, decimals=0) => Number(`${Math.round(`${n}e${decimals}`)}e-${decimals}`);
// round(1.005, 2) -> 1.01
```


### 标准差(standardDeviation)

返回一组数字的标准差.

用 `Array.reduce()` 计算平均值、方差和放长的总和，然后计算标准差.
你可以省略第二个参数来获得样本标准差或将其设为true来获得总体标准差.

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
