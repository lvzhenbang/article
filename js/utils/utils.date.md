# 常用日期功能函数

定义是获取的本地时间还是` UTC `时间：

```
isUTC = false; // 默认为非UTC
```

## 获取日期

### getYear

返回指定` date对象 ` date第几` 年 `

```
function getYear (date, isUTC) {
  return isUTC ? date.getUTCFullYear() : date.getFullYear()
}
```

### getMonth

返回指定` date对象 ` date第几` 月 `

```
function getMonth (date, isUTC) {
  return isUTC ? date.getUTCMonth() : date.getMonth()
}
```

### getDate

返回指定` date对象 ` date某月的第几` 天 `

```
function getDate (date, isUTC) {
  return isUTC ? date.getUTCDate() : date.getDate()
}
```

### getHour

返回指定` date对象 ` date第几` 小时 `

```
function getHour (date, isUTC) {
  return isUTC ? date.getUTCHours() : date.getHours()
}
```

### getMinutes

返回指定` date对象 ` date第几` 分钟 `

```
function getMinutes (date, isUTC) {
  return isUTC ? date.getUTCMinutes() : date.getMinutes()
}
```

### getSeconds

返回指定` date对象 ` date第几` 秒钟 `

```
function getSeconds (date, isUTC) {
  return isUTC ? date.getUTCSeconds() : date.getSeconds()
}
```

### getMilliseconds

返回指定` date对象 ` date第几` 毫秒 `

```
function getMilliseconds (date, isUTC) {
  return isUTC ? date.getUTCMilliseconds() : date.getMilliseconds()
}
```

### getDay

返回指定` date对象 ` date 是` 星期几 `

```
function getDay (date, isUTC) {
  return isUTC ? date.getUTCDay() : date.getDay()
}
```

### getQuarter

返回指定` date对象 ` date 是` 第几季度 `

```
function getQuarter(date, isUTC) {
  return Math.floor((getMonth(date, isUTC) / 4)) + 1;
}
```

### getDayInMonth

返回某年` year `第几个月` month `有几天

```
function getDayInMonth(year, month) {
  return /8|3|5|10/.test(month) ? 30 : month === 1 ? (!isLeapYaer(year)) ? 29 : 28 : 31
}
```

注：[` isLeapYaer(year) `](https://github.com/lvzhenbang/article/blob/master/js/check/js.other.md#isLeapYaer)

## getDateNow

返回自` 1970年1月1日 00:00:00 `到指定日期` date `的毫秒数。

```

function getDateNow(date) {
  return Date.now ? Date.now(date) : +(new Date(date));
}

```

## 设置日期

### setYear

设置` date对象 ` date的` 年 `为` val `, 然后返回修改后的` date `。

```
function setFullYear (date, val, isUTC) {
  return isUTC ? date.setUTCFullYear(val) : date.setFullYear(val)
},
```

### setMonth

设置` date对象 ` date的` 月 `为` val `, 然后返回修改后的` date `。

```
function setMonth (date, val, isUTC) {
  return isUTC ? date.setUTCMonth(val) : date.setMonth(val)
}
```

### setDate

设置` date对象 ` date的` 天 `为` val `, 然后返回修改后的` date `。

```
function setDate (date, val, isUTC) {
  return isUTC ? date.setUTCDate(val) : date.setDate(val)
}
```

### setHour

设置` date对象 ` date的` 小时 `为` val `, 然后返回修改后的` date `。

```
function setHour (date, val, isUTC) {
  return isUTC ? date.setUTCHours(val) : date.setHours(val)
}
```

### setMinutes

设置` date对象 ` date的` 分钟 `为` val `, 然后返回修改后的` date `。

```
function setMinutes (date, val, isUTC) {
  return isUTC ? date.setUTCMinutes(val) : date.setMinutes(val)
}
```

### setSeconds

设置` date对象 ` date的` 秒钟 `为` val `, 然后返回修改后的` date `。

```
function setSeconds (date, val, isUTC) {
  return isUTC ? date.setUTCSeconds(val) : date.setSeconds(val)
}
```

### setMilliseconds

设置` date对象 ` date的` 毫秒 `为` val `, 然后返回修改后的` date `。

```
function setMilliseconds (date, val, isUTC) {
  return isUTC ? date.setUTCMilliseconds(val) : date.setMilliseconds(val)
}
```

### setDay

设置` date对象 ` date 的` 星期几 `为` val `, 然后返回修改后的` date `。

```
function setDay (date, val, isUTC) {
  return isUTC ? date.setUTCDay(val) : date.setDay(val)
}
```

## 格式化日期

输入一个` date `对象，然后返回一个` YYYY-MM-DD hh:mm:ss `格式的输出

```
function formatDate (date, format) {
  var year = getYear(date);
  var month = getMonth(date) + 1;
  var day = getDate(date);
  var hour = getHour(date);
  var minute = getMinutes(date);
  var second = getSeconds(date);

  return format
    .replace(/YY/, ('' + year).slice(2))
    .replace(/MM/, ('0' + month).slice(-2))
    .replace(/DD/, ('0' + day).slice(-2))
    .replace(/hh/, ('0' + hour).slice(-2))
    .replace(/mm/, ('0' + minute).slice(-2))
    .replace(/ss/, ('0' + second).slice(-2))
}
```

注：` getYear `，` getMonth `，` getDate `，` getHour `，` getMinutes `，` getSeconds `參考上述自定义函数。

## 其他

### startOfWeek

获取给定` date `对象` val `所在` 周 `的第一天(MonDay)所指的新对象。

```
function startOfWeek(val, start) {
  const date = val.getDate();
  const day = val.getDay();
  const distanceDate = day < start ? start - day -7 : start - day;
  val.setDate(date + distanceDate);
  // 时分秒是相同
  val.setHours(0);
  val.setMinutes(0);
  val.setSeconds(0);
  val.setMilliseconds(0);
  return val;
}
```


### endOfWeek

获取给定` date `对象` val `所在` 周 `的最后一天(SunDay)所指的新对象。

```
function endOfWeek(val, start) {
  const date = val.getDate();
  const day = val.getDay();
  const distanceDate = day < start ? start - day - 1 : 6 + (start - day);
  val.setDate(date + distanceDate);
  // 时分秒是相同
  val.setHours(23);
  val.setMinutes(59);
  val.setSeconds(59);
  val.setMilliseconds(999);
  return val;
}
```

## 插件

* [` moment `](https://github.com/moment/moment)
* [` date-fns `](https://github.com/date-fns/date-fns)
* [` dayjs `](https://github.com/iamkun/dayjs)

> datepicker

* [` daterangepicker `](https://github.com/dangrossman/daterangepicker)
* [` bootstrap-datepicker `](https://github.com/uxsolutions/bootstrap-datepicker)
* [` jquery datetimepicker `](https://github.com/xdan/datetimepicker)
* [` vue-datepicker `](https://github.com/charliekassel/vuejs-datepicker)
* [` react-datepicker `](https://github.com/Hacker0x01/react-datepicker)
* [` angular-datepicker `](https://github.com/720kb/angular-datepicker)

注：`vue`, `angular/js`, `react`可以查找相关的组件库
