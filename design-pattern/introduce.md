## 什么是设计模式

设计模式这个东西很抽象，不好理解，但却很好用。

我们可以先不用试着一下子弄明白设计模式的第一，我们可以从设计模式的用途、好处、组成、分类、在一些流行框架或库中的主要体现等方面入手，一步一步的深入了解，弄懂设计模式到底是什么东东。

### 设计模式的用途

就好像工具的产生是为了解放人的生产力一样，在我们现在的应用程序世界里设计模的出现是为了解放程序员的生产力。

在开发中用来解决代码的重用问题，减少代码量;
在开发中被当作模板使用，尤其在使用场景差异较小时侯（可以在一个项目中，也可以在不同的项目中）。

### 设计模式的好处

使用设计模式的好处：

* 提升团队的协作能力；
* 提高开发效率；
* 规范程序员的行为；
* 便于程序的维护；
* 提高程序的性能；

设计模式体现出的好处：

1.设计模式有很强的表现力；

我们在看一个设计模式时，它会呈现给我们一个结构和相关的语法定义，可以帮助开发人员快速的做出问题的解决方案。

2.设计模式本身就是解决方案；

设计模式是解决软件开发中一些问题的固定方法，它用了经过时间验证的技术，这些技术反映了开发人员的经验和见解。

3.设计模式可重用；

一个设计模式就是一个现成的解决方案，可以适应我们的需要，而且本身很稳定。

注：设计模式虽好，但它不是万能的，它不能解决开发中的所有问题，也不能取代一个好的开发人员，然而掌握它的开发人员在工作中将如鱼得水。

### 设计模式的组成

#### 一个设计模式之所以被人认可，一般具有以下特点：

1. 解决特定的问题
2. 解决这个问题的方案可能不太明显，正因为不明显，它才更有普适性
3. 概念的定义经得起推敲（不能有二义性）
4. 模式一定要要描述关系性

深层次的实现机制和结构一定要解释它的代码间的关系


#### 一个设计模式要满足以下三点要求：
	
1. Fitness of purpose - how is the pattern considered successful?

	合适的目的——如何判定这个设计模式是一个成功的设计模式?

2. Usefulness - why is the pattern considered successful?

	有用性——为什么这个设计模式被认为是成功的？

3. Applicability - is the design worthy of being a pattern because it has wider applicability? If so, this needs to be explained. When reviewing or defining a pattern, it is important to keep the above in mind.

	适用性——好的设计模式的价值在于广泛的适用性。

设计模式是 `context`, `system forces`, `configuration` 以一定的规则方式的表现。

#### 设计模式的组件化需要由以下部分构成：

1. 设计模式的名称
2. 设计模式的描述
3. 背景描述，让用户快速了解自己的设计模式的有效性
4. 问题描述，让用户明白该设计模式可以用来干什么
5. 解决方案，描述问题如何解决和实现的步骤
6. 设计描述用户交互时的行为
7. 实现
8. 可视化图表展示
9. 实例代码
10. 其它需求
11. 和其他设计模式的关系
12. 用法
13. 讨论


注：一个设计模式就是一个最佳的实践

作为一个程序老手都知道，一个应用的最大的挑战在两个阶段，即是开发阶段和维护阶段，而一个好的设计模式将是避免开发事故和开发人员工作量的保证，虽然它不能避免所有的问题，但它会大大的降低问题的出现和避免常见问题的出现。

#### 反设计模式的常见表现形式：

1. 污染全局命名空间，定义了大量的全局变量
2. 传一个字符串到 `setTimeout` 或者 `setInterval`的内部用 `eval()` 作为触发器。
3. 修改 `Object` 类原型
4. 用不灵活的内联形式的JavaScript
5. 使用document.write而非更合适的document.createElement，不仅是操作效率，对dom的友好性上也是不如后者，例如：XHTML


### 设计模式的分类

设计模式分为创建型、结构行、行为型

构造器模式（Constructor Pattern）[https://github.com/lvzhenbang/article/blob/master/design-pattern/constructor.md](https://github.com/lvzhenbang/article/blob/master/design-pattern/constructor.md)

* 模块设计模式 （Module Pattern）
* 揭示模块设计模式 （Revealing Module Pattern）
* 单例设计模式 （Singleton Pattern）
* 观察者设计模式 (Observer Pattern)
* 中介者设计模式（Mediator Pattern）
* 原型设计模式（Prototype Pattern）
* 命令式设计模式（Command Pattern）
* 外观设计模式（Facade Pattern）
* 工厂设计模式（Factory Pattern）
* Mixin设计模式（Mixin Pattern）
* 装饰者设计模式（Decorator Pattern）
* 享元设计模式（Flyweight Pattern）

### jQuery使用的设计模式

* 组合式设计模式（Composite Pattern）
* 适配器设计模式（Adapter Pattern）
* 外观设计模式（Facade Pattern）
* 观察者设计模式（Observer Pattern）
* 迭代器设计模式（Iterator Pattern）
* 延迟初始化设计模式（Lazy Initialization Pattern）
* 代理设计模式（Proxy Pattern）
* 构建者（Builder Pattern）

### MV* 设计模式（前后端分离）

* MVC设计模式
* MVVM设计模式
* MVP设计模式

### 模块化的JavaScript设计模式

* AMD 设计模式
* CommonJS 设计模式
* ES Harmony 设计模式

各个设计模式详解，未完待续...