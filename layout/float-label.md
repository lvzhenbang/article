## 浮动的label

在web项目中，有一个很重的模块就是登陆/注册模块，这个模块的主体部分就是一个form表单，这个form表单包含两个重要input组（用户名/密码），每个input组都包含label和input，而关于 `label+input` 的布局方案多种多样，不同的设计师有不同的设计风格，不同的前端工程师又有不同的实现方式。通过对比发现，现在的方案是既注重美观，又注重性能。

那么，关于label和input都有哪些布局方案呢?

### label+input的布局方案

1. 将label和input（palcholder关键字提示）分为上下两部分; // 很久以前采用，现在偶尔使用
2. 将label和input（palcholder关键字提示）分为左右两部分（label占据一定的宽度，而label中的字体采用左对齐，右对齐，两端对齐这三种常见的方案）; // 案例：微博登陆，jd wap登陆页面等
3. label和input（palcholder关键字提示）还是分为左右两部分，不同的是label中的字体使用图标代替; // 案例：segment fault社区登陆页面等
4. 只包含input（palcholder关键字提示）; // 案例：手淘的登陆页面，掘金开发社区的登陆页面等
5. 只显示input（palcholder关键字提示），而label采用浮动并隐藏，当触发input的焦点事件时label显示。 // 案例：手淘的之前登陆页面，或者参考[JVFloatLabeledTextField](https://github.com/jverdi/JVFloatLabeledTextField)等

这几种方案各有优劣，使用label字体用图标代替更形象，但是增加了图标的url访问；label的中的字体个数不一致，看起来不美观，字数相同，这种方案只能说中规中矩；而直接丢弃label，可以使界面简洁美观，但是label有label的作用（下面会详细说label和placeholder的作用）；使用浮动的label，增加了动画效果，但需要引入js，这种方案性能，比起不使用js的明显要高（有一种不用js，但兼容性不是太好的方案）。

### label vs placholder

label: 描述表单元素的角色，用来指定input的是唯一字段名称

placeholder: 它提示用户输入内容的格式

它们两个看似类似，但是它们的职责不同，很多同学在这里犯了比较大的错误。

如果需要知道它们更多的内容可参考[MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/input#The_%3Clabel%3E_element)

### 带动画的label(no-js)

在做用户交互的页面时，带上动画的用户交互，往往更容易打动用户。下面就介绍一个用伪类实现的浮动label。

HTML代码：

```
<div class="input-group">
    <input type="text" id="userName" placeholder="用户名/邮箱/卡号">
    <label for="userName">账号</label>
</div>
```

基本布局css代码：

```
.input-group {
    position: relative;
    margin: 100px 20px;
    font-size: 16px;
}

.input-group>input {
    display: block;
    box-sizing: border-box;
    width: 100%;
    padding: 16px;
    font-size: 16px;
    line-height: 1.0;
    border: none;
    border-bottom: 1px solid #cdcdcd;
    border-radius: 0.4em;

    transition: box-shadow 0.3s;
}

.input-group input::placeholder {
  color: #cdcdcd;
}

.input-group>input:focus {
    outline: none;
    box-shadow: 0.2em 0.8em 1.6em #cdcdcd;
}

.input-group>label {
    position: absolute;
    bottom: 50%;
    left: 0;
    z-index: -1;
    
    visibility: hidden;
    color: #050505;
    opacity: 0;
}
```

首先，设置了 `label` 的位置（posiion: absolute），并定义了它的层级（z-index: -1）， 显隐性为（visibility: hidden），透明度（opacity: 0）;

然后，设置了input的 `placeholder`样式，可使用伪元素 `::placeholder` 设置其样式；

最后，设置了一个过渡动画效果，当input元素标签获得焦点时，使用伪类 `:focus` 定义了input元素标签获得焦点时的阴影样式（box-shadow）和轮廓样式（outline）。

label浮动效果样式

```
 .input-group>label {
      ...

      -webkit-transform-origin: 0 0;
              transform-origin: 0 0;
      -webkit-transform: translate3d(0, 0, 0) scale(0);
              transform: translate3d(0, 0, 0) scale(0);
      -webkit-transition:
          opacity 0.3s,
          visibility 0.3s,
          transform 0.3s,
          z-index 0.3s;
          
              transition:
                  opacity 0.3s,
                  visibility 0.3s,
                  transform 0.3s,
                  z-index 0.3s;
 }

.input-group>input:focus ~ label {
    z-index: 1;
    visibility: visible;
    opacity: 1;
    -webkit-transform: translate3d(0, -36px, 0) scale(1);
        transform: translate3d(0, -36px, 0) scale(1);
}
```

在定义 `label` 样式的集合内，添加它的初始 `transform` 形变效果，同时设置 `transition` 过渡效果样式 ，然后定义当 `input` 获得焦点时，它的兄弟元素 `label` 的样式即可。


这种label浮动的效果和[JVFloatLabeledTextField](https://github.com/jverdi/JVFloatLabeledTextField)的效果不同，前者是获取到焦点，label立马开始浮动，而后者是当用户输入内容时（也就是placeholder消失时），label开始浮动。要使两者的效果相同，我们可以使用伪类可以嵌套的特性，修改 `.input-group>input:focus ~ label` 为 `.input-group>input:focus:not(:placeholder-shown) ~ label` ，这里的 `:placeholder-shown` 可以定义 `placeholder` 的显隐效果，但它的兼容性不太好，ie/edge 压根不支持，chrome和safari，以及Firefox还可以，具体可参考[can i use](https://caniuse.com/#search=placeholder)。更多伪类和伪元素知识点总结，可参考[pseudo](https://github.com/lvzhenbang/pseudo)

### 案例展示

[demo](https://codepen.io/lvzhenbang/pen/yRowVE)

