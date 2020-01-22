# 设计模式七大原则

[` 介绍 `](https://www.youtube.com/watch?v=HLFbeC78YlU&list=PL6n9fhu94yhXjG1w2blMXUzyDrZ_eyOme&index=2&t=3s)

## 描述

* 单一职责原则

一个程序只做好一件事。如果功能过于复杂则拆分，拆分后的各部分保持独立

[` 视频 `](https://www.youtube.com/watch?v=hGf2upfDpdo&list=PL6n9fhu94yhXjG1w2blMXUzyDrZ_eyOme&index=2)

* 里氏替换原则

子类替代父类。只要是父类能出现的地方，子类都能出现

注：JS中使用较少

[` 视频 `](https://www.youtube.com/watch?v=gnKx1RW_2Rk&list=PL6n9fhu94yhXjG1w2blMXUzyDrZ_eyOme&index=5)

* 依赖倒置原则

面向接口编程，依赖于抽象而不是依赖于具体的实现。

注：JS中不可用，因为JS中没有接口这个概念。如果使用TypeScript，则可以使用。

注：现在的前后端分离，前端开发者不关注后端的数据如何处理，而只关注接口给出的数据。

[` 视频 `](https://www.youtube.com/watch?v=5WHKNOTqwsA&list=PL6n9fhu94yhXjG1w2blMXUzyDrZ_eyOme&index=6)

* 接口隔离原则

保持接口的单一独立，尽量避免出现‘旁接口’

注：JS中不可用，因为JS中没有接口这个概念。如果使用TypeScript，则可以使用。

[` 视频 `](https://www.youtube.com/watch?v=CWrRwC8iB30&list=PL6n9fhu94yhXjG1w2blMXUzyDrZ_eyOme&index=3)

* 迪米特原则

一个对象应该尽可能少的与其它对象间的相互作用。

* 开闭原则

对扩展开放，对修改封闭。增加需求时，扩展新代码，而非修改已有的代码。

注：这是软件设计的终极目标

[` 视频 `](https://www.youtube.com/watch?v=wo06oCBuYYI&list=PL6n9fhu94yhXjG1w2blMXUzyDrZ_eyOme&index=4)

* 组合/聚合复用原则

尽量使用组合/聚合达到复用，尽量少用继承。

## 参考资料

* [` 设计模式之七大基本原则 `](https://zhuanlan.zhihu.com/p/24614363)
