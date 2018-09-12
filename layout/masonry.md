## 瀑布流

待续...

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

### 插件

[mansonry](https://github.com/desandro/masonry)
[macy.js](https://github.com/bigbitecreative/macy.js)
[salvattore](https://github.com/rnmp/salvattore)

### 参考资料

[Generating Masonry Layouts](https://www.sitepoint.com/understanding-masonry-layout/)