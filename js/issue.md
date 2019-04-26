# js 开发中，遇到的问题

* IE不允许分配到只读属性

```
let div = document.querySelector('div');

div.style = '';
```

IE 报错：

```
strict 模式下不允许分配到只读属性。
```

解决方案：

```
document.querySelector('div').setAttribute('style', '')
```
