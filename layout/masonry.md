## 瀑布流

瀑布流包含了相当丰富的前端知识点，如：布局(传统)、js的基础的掌握（DOM, BOM, javaScript的基本语法）、grid网格布局、ajax、懒加载（lazyload）、debounce(scroll事件被频繁添加)、throttle（短时间频繁触发load more）、只对新load的项目进行瀑布流布局等

### 等宽不定高的瀑布流

> flex

[demo](https://github.com/lvzhenbang/article/blob/master/layout/mansonry/flex.html)

需要设置容器的宽和高，所有项目的高度加起来要小于容器高度，要不然，只有一列；如果高度过低，可会产生水平方向的滚动条。需要人为的控制设置合理的高度，让项目即填充满容器，又不使容器产生水平方向的滚动条。

特点：这是一种自上而下的s形循环排序。（实际开发中，很少用到）

> multiple columns

[demo](https://github.com/lvzhenbang/article/blob/master/layout/mansonry/multi-col.html)

同flex布局一样需要控制高度。

特点：这是一种自上而下的s形循环排序。（实际开发中，很少用到）

> grid

使用grid，做瀑布流效果，要注意的就是grid布局方案的特点：

1. 它是网格布局(dispaly:grid)
2. 设置要展示的列模板（grid-template-columns）
3. 项目会自动自左向右排序，需要设置grid-auto-rows，这样项目会按照添加的顺序自动找最合适的空间去填充，然后只需要设置这个项目的位置和跨度即可，可通过grid-row（grid-row-start, grid-row-end）


通过grid实现的瀑布流，需要自己控制项目的位置，也就是需要自己设置每个项目的css，在开发中手动的设置不可取（可参考demo-1），所以需要借助于JavaScript，这里相比较于传统的布局方案，会少些很多的js（可参考demo-2）

特点：这是一种从左至右的循环排序。（实际开发中，经常用到）

[demo-1](https://github.com/lvzhenbang/article/blob/master/layout/mansonry/grid.html)

[demo-2](https://github.com/lvzhenbang/article/blob/master/layout/mansonry/grid-2.html)

使用grid做瀑布流，是一件很有意思的事儿，通过它的实现，对grid的认识变得更加清晰。总结如下经验：

1. grid的项目的自动漂浮寻找合适的位置，只需要设置grid-row-end属性的span即可，不要设置grid-row-start 和 grid-row-end的定位，否则，自动漂浮的效果将消失；

2. grid的容器如果设置grid-template-columns: repeat(auto-fill, minmax(300, 1fr))，这种自动生成列的形式，且同时设置grid-gap, 那么自改变窗口大小的过程中，将会出现一些项目间没有gutter, 但是像上面demo中列的gap不受影响，受影响的是行，这是grid的局限，解决的办法还是有的：可以用grid-column-gap定义列的，行的我们通过设置grid-auto-rows的值，这样就可以实现分别定义行的和列的，然后，还需要设置grid-row: span n; 这样让项目容器多合并一行，然后设置项目容器中的内容align-items: center; 即可，最后，设置第一的正常合并即可。

3. 如果grid-template-columns: repeat(auto-fill, minmax(300, 1fr))这样的列自动生成，如何秋猎的个数，暂时我没有好的办法，只能用窗口的宽度/列宽度，结果向下取整。（补充：grid-template-columns: repeat(3, 1fr); 则可以通过 grid.style.gridTemplateColumns.split(' ').length，求列的个数 ）

代码参考如下demo:

[demo-3]((https://github.com/lvzhenbang/article/blob/master/layout/mansonry/grid-3.html)

### 如何做一个优秀的瀑布流

一个beautiful的瀑布流就应该是这样的，最好采取浏览器支持的dom渲染方式，如果浏览器支持瀑布流这种布局的方式，就不需要写它的布局代码，但某人为现在不太可能，但将来说不定，有人直接提议支持display: masonry; 这里不做引申了，虽然flex和multi-col可以实现类似的布局，但这种布局并不是严格意义上来说的masonry，但使用grid外加少量的js，就可以轻松的实现一个瀑布流。

言归正传，那么如何做一个优秀的瀑布流呢，正如文章一开始说的那样，要对它的功能进行优化。

1. 预加载（让页面预先加载几个item）
2. 懒加载（lazyload，针对图片的处理）
3. debounce(scroll事件被频繁添加)
4. throttle（短时间频繁触发load more）
5. 只对新load的项目进行瀑布流布局
6. 新添加的item批量加载的优化（for循环——>fragment——>template）
7. 只对新增加的item进行布局
8. 使用IntersetionObserver这个API来实现lazyload和infinite scroll

未完待续...

### 插件

[mansonry](https://github.com/desandro/masonry)
[macy.js](https://github.com/bigbitecreative/macy.js)
[salvattore](https://github.com/rnmp/salvattore)

### 参考资料

[Generating Masonry Layouts](https://www.sitepoint.com/understanding-masonry-layout/)

[Debouncing and Throttling Explained Through Examples](https://css-tricks.com/debouncing-throttling-explained-examples/)

[IntersectionObserver API 使用教程](http://www.ruanyifeng.com/blog/2016/11/intersectionobserver_api.html)

[ntersection Observer comes to Firefox](https://hacks.mozilla.org/2017/08/intersection-observer-comes-to-firefox/)