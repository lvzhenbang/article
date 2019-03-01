# jquery 总结

```
<div id="main" class="main">
  <div class="first-div" frist> first div </div>
  <div class="twice-div" two> twice div </div>
  <p class="first-parg"> first pargraph </p>
  <p class="twice-parg"> twice pargraph </p>
</div>

<input type="text" placeholder="请输入姓名">
<input type="password" placeholder="请输入密码">
```

## 获取一个元素对象

1. 根据选择器

```
$('#main')
$('.main')
$('.main div')
$('.main:first-child')
$('.p:last-of-type')
$('div[first]')
$('.input[type="text"])
```

2. 根据范围

```
$('div').eq(2)
$('#main').find('p')
// 前驱/后继
$('.first-parg').prev()
$('.first-parg').next()
// 兄弟元素
$('.first-parg').siblings()
$('.first-parg).prevAll()
$('.first-parg).nextAll()
```

当还有一些其他的API如：`children`, `parent`, `parents`等。

注：jquery中判断一个对象是否存在，是判断返回对象的length属性的值是否等于0，如果返回的对象的length的值为0，则要查找的对象不存在；如果大于等于1，则存在。

## 向一个对象中添加对象

```
$('.first-div').prepend('<span>hello div</span>')
$('.first-parg').append('<span>hello p</span>')
```

当然，我们也可以用`.html()`，如果该方法传入参数，那么对象所对应的值将会被替代，如果不传入参数，那么就是获取对象的内容。

## 向一个对象添加兄弟对象

```
$('.first-div').before('<div class="zero-div"> zero div</div>')
$('.first-div').after('<div class="last-div"> last div</div>')
```

## 清空对象

`empty()` 清空所有的子元素，`remove()` 清除所有子元素，包括它自身。


