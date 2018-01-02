## 设计模式

‘从大处着眼，从小处着手’，以前对这句话一知半解，自从踏出校门走入社会，开始工作以来，有了越来越深的理解，偶有发现这句话用在程序开发中也有用，所以，近段时间开始尝试着分析jQuery源码，分析angularjs源码，学习设计模式。

### 设计模式的由来

看过GOF的总结的23种设计模式的人，都或多或少的有种似曾相识的感觉，事实确实如此，这些设计模式原来就有，是前人优秀的工作成果，只不过是GOF他们给这些原本就有的东西重新定义了一下，给予这些东西名称和原理，使之更容易被人理解和接受，这本身就体现了GOF的伟大，让好的东西更容易传播。

### 设计模式的定义

在面向对象软件设计过程中针对特定问题的简洁而优雅的解决方案。

### 如何学习设计模式

设计模式也并不是什么洪水猛兽，高不可攀，一个有一定经验的软件开发者都会在不知不觉中使用它，这可能中间经历了很多的尝试，当他看过设计模式后，会发现原来已经有人对它进行过总结。

使用设计模式实现的代码，使用一般的方法都能实现，所以使用设计模式，会无形中增加代码的量，尤其是不正确的使用，更会带来毁灭性的灾难，所以，一般的开发人员唯恐避之不及。

### 理解‘可复用的面向对象软件基础’

设计模式的实现都遵循一条原则‘找出程序中变化的地方，并将它封装起来’。在程序设计中总分为可变的地方和不可变的地方，可变的地方我们往往将他封装起来，不可变的地方也即是代码稳定和不可变的部分的，往往这部分代码是可复用的。这也是标题《可复用的面向对象软件基础》的由来。

好了，废话不多说，下面进入常见的设计模式学习。

### 目录

[‘大处着眼，小处着手’——设计模式系列](https://github.com/lvzhenbang/article/blob/master/design-pattern/introduce.md)

注：这是我个人对设计模式的认识和理解，仅代表个人观点个看法，不足之处欢迎大家指正，随着认识的加深这篇文章（包括设计模式其他系列文章）会不断地进行更新。

### javascript 设计模式
	
[构造器 设计模式](https://github.com/lvzhenbang/article/blob/master/design-pattern/constructor.md)

[外观 设计模式](https://github.com/lvzhenbang/article/blob/master/design-pattern/facade.md)

[工厂 设计模式](https://github.com/lvzhenbang/article/blob/master/design-pattern/factory.md)

[观察者 设计模式](https://github.com/lvzhenbang/article/blob/master/design-pattern/observer.md)

[单例 设计模式](https://github.com/lvzhenbang/article/blob/master/design-pattern/singleton.md)

持续更新中...

### 参考资料

[学习JavaScript设计模式](https://addyosmani.com/resources/essentialjsdesignpatterns/book/)

[腾讯全端 AlloyTeam 设计模式系列文章](http://www.alloyteam.com/2012/10/common-javascript-design-patterns/)