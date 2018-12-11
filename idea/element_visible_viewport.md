# 判断DOM元素是否出现再浏览器窗口中

几乎所有的项目都要解决这样一个问题：判断一个元素是否出现在浏览器窗口中？因为通过它我们可以极大的优化项目的性能，进而提升用户的的体验。

## 使用场景及技术分析

所涉及的业务实现，比较常见的就是电商平台或者是图片展示类的网站。电商网站，如：[淘宝](https://www.taobao.com/)、[京东](https://www.jd.com/)等；图片展示类，如：[花瓣](http://huaban.com/)，[pinterest](https://www.pinterest.com/)。

涉及的技术，如：[lazyload](https://baike.baidu.com/item/lazyload/7877218?fr=aladdin)技术动态的加载图片（元素），无限加载技术，包括基于骨架屏技术加载静态资源。

* 懒加载（lazyload）：它目的是按需加载，而很大一部分项目的前端实现是通过判断一个元素是否出现在浏览器窗口中，如果出现则将img元素标签内的src属性中的图片地址替换成自定`data-src`属性的地址，但这里不一定是`data-src`属性，也可能是`srcset`，[pinterest](https://www.pinterest.com/)中就是这样做的，当然你也可以定义成任何你喜欢的，这只是其中的一种方式；另一种是判断元素是否出现在浏览器窗口中之后，然后加载一个HTML代码块，[小米商城官网](https://www.mi.com)就是这样实现的，当然也有其他一些也是这样做的，这里就不一一做介绍了。

* 无限加载（infinte scroll）：它是通过底部的 `加载更多` 这个代码块是否出现在浏览器窗口中，如果在，就向容器代码块中追加一定数量的相关代码块，这时 `加载更多` 这个代码块就会被挤压出到浏览器窗口之外。当然有些无限加载技术也使用lazyload。

* 骨架屏技术（skelton screen）：这个技术在前一波广受关注，它的原理就是在请求一个页面时，先不显示页面的内容，先显示页面的布局，像文字、图片、视频等静态资源都不显示，而显示的是与页面布局相关的css样式。其实通俗一点来说，就是网页做一个CT，这样是不是理解起来更方便？像淘宝的pc官网，youtube都是这样实现的。

这些业务和技术的目的都是为了为了解决低网速情况下使用web应用，如果页面内内容没有加载，那么就会造成低用户体验。

这些业务和使用的技术基本上都用了 `判断页面的元素是否出现在浏览器窗口中`。

## 如何判断一个元素是否出现在窗口中呢？

> 现在通用的是基于浏览器的窗口的判断

那么何为基于浏览器窗口的判断呢？这要通过DOM的API `.getBoundingClientRect()` ，获取目标元素距离浏览器窗口的位置坐标(top, left) 或者（x, y）坐标，所以说是基于浏览器窗口的。我们可以拖动浏览器的滚动条来使目标元素从浏览器的顶端进入浏览器窗口（这可以判断上边界），也可以从浏览器的底部进入浏览器窗口（这可以判断下边界），而这正好是判断目标元素进入浏览器窗口的边界。

上边界：

目标元素的底边刚好和浏览器的顶部重合，当滚动条向下滚动，目标元素从底部开始一点点的出现，直到目标元素整个出现在浏览器窗口，反之，则逐渐远离。如果目标元素高度大于浏览器窗口的高度，那么浏览器窗口内就不不出现整个目标元素，而只会出现部分，基于这种情况衍生了一种目标元素背景图片的滚动动画。可参考[苹果官网](https://www.apple.com/cn/apple-watch-series-4/)。

下边界：

目标元素的顶部刚好和浏览器的底部重合，当滚动条向上滚动，目标元素从顶部部开始一点点的出现，直到目标元素整个出现在浏览器窗口，反之，则逐渐远离。

我们可以浏览器的滚动条向下滚动(也可以按照向下滚动，答案类似)写出如下的代码：

```
var clienRect = el.getBoundingClientRect();

if (clientRect.top > -clientRect.width && clientRect.top < window.innerHeight) {}
// 或者
if (clientRect.bottom > 0 && clientRect.top =< window.innerHeight) {}
```

有的同学可能会问，是不是窗口的高度小于目标元素的高度，或者窗口的高度大于目标元素的高度都这样都一样呢？我想说一样，因为这里是根据两个边界得出的结论。

上面的判断包括目标元素的部分出现在浏览器窗口，那么如何判断整个目标元素出现在浏览器的窗口中呢？可参考如下的代码：

```
var clienRect = el.getBoundingClientRect();

if (clientRect.top > 0 && clientRect.top < window.innerHeight - clientRect.width) {}
// 或者
if (clientRect.top >= 0 && clientRect.bottom > window.innerHeight) {}
```

兼容性可参考[can i use](https://caniuse.com/#search=getBoundingClientRect)

> 基于document文档的顶部来判断

这里要使用的是DOM元素的 `.offsetTop` 来计算目标元素的顶部边界到document文档的顶部边界的距离，使用window的 `.pageYoffset` 来计算当浏览器滚动条滚动时document文档卷起的高度，通过比较这两个高度，我们就可以轻松的判断目标元素是否出现在浏览器窗口内。

当目标元素的上边界和浏览器窗口的下边界重合，目标元素的 `offsetTop` 是个定值，所以此时document文档卷起的高度和目标元素的 `offsetTop` 之间存在这样一个等式：`window.innerHeight + window.pageYoffset = el.offsetTop`。若继续向下滚动浏览器的滚动条，目标元素将出现在浏览器当中，我们都知道 `window.pageYoffset` 的值将继续变大，由于 `el.offsetTop` 和 `window.innerHeight` 都是个定值，所以我们可以得到这样一个边界条件，当 `window.pageYoffset > el.offsetTop - window.innerHeight` ，目标元素从浏览器窗口的底部逐渐出现。

当浏览器的滚动条继续向上滚动会出现目标元素的下边界和浏览器窗口的上边界重合的情况，当继续向下滚动滚动条元素将从浏览器窗口消失，此时存在这样一个等式：`window.pageYoffset + el.offsetHeight = el.offsetTop` ，所以很明显，如果 `window.pageYoffset` 的值继续增大，目标元素将消失与浏览器窗口，因此，很明显，只有 `window.pageYoffset < el.offsetTop - el.offsetHeight` 目标元素才会出现在浏览器窗口内。

参考代码如下：

```
if (window.pageYoffset > el.offsetTop - window.innerHeight && window.pageYoffset < el.offsetTop - el.offsetHeight) {}
```

当然，上面的是目标元素部分或整体出现在浏览器窗口的判断条件，如果是整个目标元素，方法同上，可参考下面的代码：

```
if (window.pageYoffset > el.offsetTop - window.innerHeight - el.offsetHeight && window.pageYoffset < el.offsetTop) {}
```

虽然这种方法也可以，但它的浏览器兼容性不如第一种。

## 参考资料

* [How to tell if a DOM element is visible in the current viewport?
](https://stackoverflow.com/questions/123999/how-to-tell-if-a-dom-element-is-visible-in-the-current-viewport)
* [Lazy Loading Images](https://css-tricks.com/snippets/javascript/lazy-loading-images/)
* [jquery lazyload 插件](https://github.com/tuupola/jquery_lazyload#basic-usage)
* [infinite-scroll 插件](https://github.com/metafizzy/infinite-scroll)
* [scroll-magic 插件](https://github.com/janpaepke/ScrollMagic)
* [Building Skeleton Screens with CSS Custom Properties](https://css-tricks.com/building-skeleton-screens-css-custom-properties/)