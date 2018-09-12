## grid layout

作为W3C最新提出来的布局方案，它的优势不言而喻。它把原先基于 `display + position + padding` 的实现的栅格化布局，通过简单的代码来实现，通过bootstrap的广泛流行，我们就可以看的出来。通过使用grid布局，我们可以轻松的实现瀑布流布局，此外，我们也可以通过css来改变html某一模块的位置，而这些是之前布局方案所不具备的。那么，说到这里，我们就不得不说以下，flex-box这种布局方案，它现在已经得到了广泛的浏览器支持，在bootstrap.4.x中已经开始使用它来进行页面的布局方案，完全抛弃了`display + position + padding`，那么，作为W3C最新推出的布局方案，它是不是flex-box的替代方案呢？有专业人士说不是，通过我个人的使用经历，我发现grid layout在处理局部网页内容的布局堪称神器，在用它进行整体的布局，它也是那么出色，这使我很难相信它不是flex-box的替代方案，因为flex box能够实现的，grid layout基本都能实现，要真的说grid layout不能实现flex box的功能就是felx box对项目控制，如：项目伸缩（flex-grow, flex-shrink），项目的反转（flex-wrap: reverse）, 项目是列排序，还是行排序...

### 知识点

和flex box的分类类似，grid layout 也分为容器和项目属性

> 容器：容器又分为列属性，行属性和间隙属性

列属性：grid-template-columns（支持所有常见的单位，包括它自己新引入的fr，fr意思就是均为），justify-content

行属性：grid-template-rows, align-content

间隙属性：grid-gap

此外，还有一个grid-template-areas，使用它可以给每个单元格指定名字和所属关系。

当然，我们还可以使用repeat() 和 minmax()，来简写属性之代码，主要适用于（grid-template）


> 项目

grid-row：1/ span 3;(grid-row: 1/4) 属性值 `1/` 代表该项目所在的起始位置，为第一行，`span 3` 表示该项目横向跨度为三个单元格

grid-column：1/ span 3;(grid: 1/4)

通过grid-row和grid-column就可以定义一个项目的位置和大小

grid-area: 1 / 2 / 3 / 4; 意思是起始位置是第一行，第二列，行跨度为2，列跨度为3。



### 学习资料

[w3cschool grid](https://www.w3schools.com/css/css_grid.asp)

[Getting Started with CSS Grid Layout
](https://scotch.io/tutorials/getting-started-with-css-grid-layout)

[一个关于grid layout的小游戏](https://www.gridcritters.com)