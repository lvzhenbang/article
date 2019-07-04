# 常用日期功能函数

定义是获取的本地时间还是` UTC `时间：

```
isUTC = false; // 默认为非UTC
```

## 获取日期

### getYear

返回指定` date对象 ` date第几` 年 `

```
function getFullYear (date, isUTC) {
  return isUTC ? date.getUTCFullYear() : date.getFullYear()
},
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

```
formatDate (date, format, lang) {
  lang = (!lang) ? en : lang
  var year = this.getYear(date)
  var month = this.getMonth(date) + 1
  var day = this.getDate(date)

  return format
    .replace(/dd/, ('0' + day).slice(-2))
    .replace(/d/, day)
    .replace(/yyyy/, year)
    .replace(/yy/, String(year).slice(2))
    .replace(/MMMM/, this.getMonthName(this.getMonth(date), lang.months))
    .replace(/MMM/, this.getMonthNameAbbr(this.getMonth(date), lang.monthsAbbr))
    .replace(/MM/, ('0' + month).slice(-2))
    .replace(/M(?!a|ä|e)/, month)
    .replace(/su/, this.getNthSuffix(this.getDate(date)))
    .replace(/D(?!e|é|i)/, this.getDayNameAbbr(date, lang.days))
}
```