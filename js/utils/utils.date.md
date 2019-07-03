# 常用日期功能函数

定义是获取的本地时间还是` UTC `时间：

```
isUTC = false; // 默认为非UTC
```

## 获取日期

### getYear

返回指定` date对象 `第几` 年 `

```
function getFullYear (date) {
  return isUTC ? date.getUTCFullYear() : date.getFullYear()
},
```

### getMonth

返回指定` date对象 `第几` 月 `

```
function getMonth (date) {
  return isUTC ? date.getUTCMonth() : date.getMonth()
}
```

### getDate

返回指定` date对象 `某月的第几` 天 `

```
function getDate (date) {
  return isUTC ? date.getUTCDate() : date.getDate()
}
```

### getMinutes

返回指定` date对象 `第几` 分钟 `

```
function getMinutes (date) {
  return isUTC ? date.getUTCMinutes() : date.getMinutes()
}
```

### getSeconds

返回指定` date对象 `第几` 秒钟 `

```
function getSeconds (date) {
  return isUTC ? date.getUTCSeconds() : date.getSeconds()
}
```

### getMilliseconds

返回指定` date对象 `第几` 毫秒 `

```
function getMilliseconds (date) {
  return isUTC ? date.getUTCMilliseconds() : date.getMilliseconds()
}
```

### getDay

返回指定` date对象 ` 是` 星期几 `

```
function getDay (date) {
  return isUTC ? date.getUTCDay() : date.getDay()
}
```

### getDayInMonth

返回某年` year `第几个月` month `有几天

```
function getDayInMonth(year, month) {
  return /8|3|5|10/.test(month) ? 30 : month === 1 ? (!(year % 4) && year % 100) || !(year % 400) ? 29 : 28 : 31
}
```

## 设置日期

### setYear

设置` date对象 `的` 年 `为` val `, 然后返回修改后的` date `。

```
function setFullYear (date, val) {
  return isUTC ? date.setUTCFullYear(val) : date.setFullYear(val)
},
```

### setMonth

设置` date对象 `的` 月 `为` val `, 然后返回修改后的` date `。

```
function setMonth (date, val) {
  return isUTC ? date.setUTCMonth(val) : date.setMonth(val)
}
```

### setDate

设置` date对象 `的` 天 `为` val `, 然后返回修改后的` date `。

```
function setDate (date, val) {
  return isUTC ? date.setUTCDate(val) : date.setDate(val)
}
```

### setMinutes

设置` date对象 `的` 分钟 `为` val `, 然后返回修改后的` date `。

```
function setMinutes (date, val) {
  return isUTC ? date.setUTCMinutes(val) : date.setMinutes(val)
}
```

### setSeconds

设置` date对象 `的` 秒钟 `为` val `, 然后返回修改后的` date `。

```
function setSeconds (date, val) {
  return isUTC ? date.setUTCSeconds(val) : date.setSeconds(val)
}
```

### setMilliseconds

设置` date对象 `的` 毫秒 `为` val `, 然后返回修改后的` date `。

```
function setMilliseconds (date, val) {
  return isUTC ? date.setUTCMilliseconds(val) : date.setMilliseconds(val)
}
```

### setDay

设置` date对象 ` 的` 星期几 `为` val `, 然后返回修改后的` date `。

```
function setDay (date, val) {
  return isUTC ? date.setUTCDay(val) : date.setDay(val)
}
```
