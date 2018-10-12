## 对话框

对话框（别称模态框，浮层）是web项目中用于用户交互的重要部分，我们最常见的就是js中 `alert()，confirm()`，但是这个对话框的不美观，也不能自定义样式，所以在开发的过程中，一般根据自己自己的需求造轮子或者使用第三方的。

### 对话框的组成

常见的弹出框形式：

位置：屏幕的左上角，右上角，左下角，右下角，垂直居中等

大小：定宽定高，定宽不定高，不定宽不定高等

开发中的对话框形式就是位置和大小随机组合的一种情况。

但是有一种情况（不定宽高的垂直居中）不易实现（可以使用display:table或css3的translate或flex实现），具体可参考[水平垂直居中布局](https://github.com/lvzhenbang/article/blob/master/layout/html-layout.md?#水平垂直居中布局)

上面的对话框包含内容的容器，还有一个是对话框下面的遮罩层（mask），遮罩层是用户触发弹出框后，形成的一个对话框与页面主体的分割图层，它的存在可以给用户一个更明显的视觉差效果，同时也起到避免用户触发对话框后的其他不必要的页面主体操作，从而产生更有的用户体验。

### 存在问题

虽然，有很多对话框的轮子供我们选择，但是我们面临着各种各样的问题。

* 到底选择哪一种对话框（对于有选择综合症的人来说一个头疼的问题）
* 可用性（api的简单与否，是否依赖了其他第三方的库）
* 可扩展性
* 浏览器的兼容性支持

那么，有没有一个简单的方法来做做一个对话框呢？当然有，我们可以使用HTML5的 `dialog` 元素。

### HTML5(dialog)

```
<dialog open>
    <h2> Hello world.</h2>
</dialog>
```

很简单，我们使用上面的代码就可以做一个弹出内容为‘Hello world.’ 的对话框。

控制对话框的显示/隐藏也很容易，添加 `open` 属性表示显示，去除为隐藏。当然，我们也可以通过DOM接口来控制 `dialog` 的显隐(show(), close())

对话框下面的遮罩层，我们可以使用 `::backgrop` 伪元素，而它的激活，我们需要使用 `showModal()` 这个DOM的API，`::backgrop` 的特性是它的位置在dialog之下，在任何 `z-index` 之上。

使用 `showModal()` 不仅可以让遮罩层显示，dialog容器也显示，所以用到 `::backdrop` 的时候，可以用 `showModal()` 代替 `show()` 这个API；如果按键盘 `ESC` 键将关闭弹出层，当然你一可以使用 `close()` 这个DOM API。

我们可以设置 `::backdrop` 这个图层为半透明图层，代码如下：

```
dialog::backdrop {
  background-color: rgba(0, 0, 0, 0.75);
}
```

除了我们常见的提示信息的弹出层外，还有一类比较使用的是带表单的弹出层。

### 带表单的弹出层

我们可以使用HTML5的dialog元素结合form元素来做这些弹出层吗？

答:可以

我们怎么做才能让form元素和dialog元素紧密的结合起来呢？

答：我们只需要在dialog元素中添加 `method="dialog"` 这个属性即可，这样就可以将button元素的属性 `value` 的值传递给dialog元素。

```
<dialog>
  <form method="dialog">
    <p>确定 or 取消</p>
    <button type="submit" value="yes">确定</button>
    <button type="submit" value="no">取消</button>
  </form>
</dialog>

<script>
    var dialog = document.querySelector('dialog')
    dialog.showModal()
    dialog.addEventListener('close', function(event) {
        console.log(dialog.returnValue)
    })
</script>
```

[demo](http://jsfiddle.net/sanlv/0pg69jyv/3/)

### 浏览器兼容性

虽然，这是一个比较好用的HTML5，但是还存在兼容性问题，chrome和opera支持的比较好，Firefox中是实验特性，但是IE，Edge, safari支持的不好，ios不支持，Android也支持的不够好，只有Android6以后支持。具体可参考[caniuse](https://caniuse.com/#search=dialog)

那么，能不能让旧版本的浏览器支持HTML5的dialog呢？首先，我们想到的是有没有一个支持dialog的polyfill，就像支持es6新特性的babel-polyfill一样，确实有这样一个开源项目[a11y-dialog](https://github.com/edenspiekermann/a11y-dialog)，它分别提供了vue和react的不同版本。