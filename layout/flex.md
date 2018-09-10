## flex

flex 是一个优秀的响应式布局方案，对于移动端来说基本不存在什么问题，但是对于pc端来说，存在很多的兼容性问题，因为flex是2009年W3C提出的方案，所以，对于一些低版本的浏览器来说，压根就不支持这种方案，各主流浏览器对flex的支持情况 ie>=10, chrome>=21, opera>=12, firefox>=22, safari>=6.1。所以，对于现在还在使用xp系统的用户来说，flex压根就不支持，特别是对于国内这些没有自主内核的浏览器厂商，兼容它们的套壳浏览器也是一个问题。

但是，对于做erp这种企业级的管理项目，使用flex将变得容易，因为我们可以给它们指定浏览器，这样就可以用最少的代码实现页面的布局，我们就告别了display + postion + padding 的布局方案。

### 认识flex

在使用前，我们需要深入细致的了解flex，flex 是flexible box 的缩写，意为弹性布局，正如字面意思，就是让盒子变得有弹性。

它包括两部分，一部分为容器，另一部分为项目。容器就是我们常用的html盒子，项目就是盒子内部的子盒子。

flex分别提供了设置容器的属性和项目的属性。

容器：flex-direction， flex-wrap, flex-flow, justify-content, align-items, align-content。其中flex-flow 是flex-direction, flex-wrap 的合写。

项目：order, flex-grow, flex-shrink, flex-basis, flex, align-self。其中flex是flex-grow, flex-shrink, flex-basis的合写。

关于它们各自的属性值可以看 [flex语法@阮一峰](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)

### 响应式开发

FlexBox 做响应式开发需要用到的属性是flex，或者更确切的说是flex-shrink, flex-grow,

如果要固定宽度，或者高度，需要用到， flex-basis配合min-width, max-width（不清楚如何使用的可以看下边学习资料）

### 学习资料

[flex-basis 和 width 的区别](http://gedd.ski/post/the-difference-between-width-and-flex-basis/)
