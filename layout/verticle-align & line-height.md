## 垂直居中对齐

在web开发中，我们会遇到各种各样的布局，比如：整体布局中的导航宽度固定，高度固定，内容区区域随着浏览器的宽度自适应的布局（这种布局在ERP项目中又有许多不同的变种），弹出窗口的水平垂直居中对齐等，局部的布局根据不同的设计风格，元素的搭配，又有不同的实现方式，这里就不多做介绍了。


在介绍今天的内容之前希望大家对vertical-align，line-height，font-size三个样式属性有充分的了解，同时了解它们彼此之间的关系。

### 知识点总结

今天要总结的知识点是垂直居中对齐，简单的方法，我们设置父元素的line-height的属性值等于高度的属性值就好了，当然，也可以考虑使用 `display:flex; align-items: center;` 来设置父元素的样式，它可以保证子元素在父元素中绝对垂直居中，而且它在子元素有多个且是不同的DOM元素节点的随机组合的情况下依然有效，但这种方式有一个缺陷，就是兼容性不好，在移动端效果不错，而在pc端效果不好，因为移动端产品更新升级的比较快。如果我们使用line-height处理多个内联（inline）元素的垂直居中的话，很困难，有一个vertical-align属性它可以设置内联元素的垂直布局，它的实现原理就是让元素可以根据父元素的位置进行排版，有top, bottom, middle三个属性值可使用，如果子元素为一个，我们使用它设置子元素样式没问题，而如果子元素为多个，我们就发现布局乱了，问题就来了，那么，该如何解决呢，vertical-align不仅提供了top,bottom,middle，也提供了length，我们可以通过它来设置。


### 自我提问（看看你对这个知识点的掌握的情况）

1. font-size的属性值是否为内容的真实高度？

2. vertical-align的原理？

3. vertical-align 和l ine-height的关系？

### 案例分析

应用常见的排版问题，但只有极少一部分解决的，

字体图标和字体混排[小米商城的导航栏最右端--购物车部分](https://www.mi.com/mi8i/)

css 代码：

```
margin-right: 4px;
font-size: 20px;
line-height: 20px;
vertical-align: -4px;
```

百度云管理平台（https://console.bce.baidu.com/）

百度只是对侧边栏导航做了处理，而且它的布局是在父元素中使用了两个span子元素，然后对这两个子元素用vertical-align: middle处理了一下。

而对于‘安全事件’模块的‘环比->持平’这样的文本+span(i)的布局则为处理，至于原因，不详，各位可以脑补。

### 参考资料 

注：强烈推荐第一篇文章。

[Deep dive CSS: font metrics, line-height and vertical-align](https://iamvdo.me/en/blog/css-font-metrics-line-height-and-vertical-align)

[我对CSS vertical-align的一些理解与认识（一）](https://www.zhangxinxu.com/wordpress/2010/05/%E6%88%91%E5%AF%B9css-vertical-align%E7%9A%84%E4%B8%80%E4%BA%9B%E7%90%86%E8%A7%A3%E4%B8%8E%E8%AE%A4%E8%AF%86%EF%BC%88%E4%B8%80%EF%BC%89/)

[CSS vertical-align的深入理解(二)之text-top篇](https://www.zhangxinxu.com/wordpress/2010/06/css-vertical-align%E7%9A%84%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3%EF%BC%88%E4%BA%8C%EF%BC%89%E4%B9%8Btext-top%E7%AF%87/)

[CSS深入理解vertical-align和line-height的基友关系](https://www.zhangxinxu.com/wordpress/2015/08/css-deep-understand-vertical-align-and-line-height/)
