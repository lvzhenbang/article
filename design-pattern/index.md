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

### 如何分辨设计模式

有时侯我们经常会遇到这样的问题，许多设计模式的实现看起来几乎一模一样，比如：代理模式和装饰者模式，策略模式和状态模式。

你不仅会大声问，他们有什么区别呢？

其实，从代码结构上看并没什么区别，就像一把手枪，你拿它来杀人,它就是凶器，你拿它来救人它就是武器。跟你的用途有关系，其实最根本的是你的意图。所以，在学习设计模式的时候，不要在在意代码的结构形式，要多留意模式的使用场景，在这种场景下解决了什么问题，多进行对比（使用前，使用后有何差别）。

### 重新审视JavaScript设计模式

JavaScript从开始被人当成为一种玩具语言，到后来发展为一门流行的可靠的语言。人们从开发做一些简单的交互，到后来Google做的第一个邮件系统，再到后来Google推出的angular框架的出现，js的威力在被人们认识的同时，伴随着浏览器支持js做更多的东西，与此同时它也变得痈肿起来，框架间各种复杂的依赖甚至能让你崩溃。随着ES6, TypeScript, CoffeScript 等各种转编译语言的兴起，无疑给前端开发者带来了不小的学习压力。像React，vue，angular这些当下流行的框架，大家都知道一些，但是要说有深入研究，不见得有多少人。17年年底我发现各个公司的在招人时，对开发人员的要求越来越高，要知道这些框架的原理，要知道某些具体的功能如何实现，同时对于设计模式的考察也越来越突出。所以，基于开发语言的使用环境，以及工作面试需要，我们不得不认真对待JavaScript常见的设计模式。

从前由于使用的局限性，和做的应用相对简单，js不被重视，js就更谈不上设计模式的问题。虽然，现在JavaScript被开发人员越来越重视，但是JavaScript设计模式的讨论还不是那么活跃，有研究和见地的还是少数人，但是研究过源码的同学就会知道，在vue，angular种设计模式已经相当普遍。

作为一个励志成为前端小牛的我，现在也甚是心痒。

### 目录

[‘大处着眼，小处着手’——设计模式系列](https://github.com/lvzhenbang/article/blob/master/design-pattern/introduce.md)

注：这是我个人对设计模式的认识和理解，仅代表个人观点个看法，不足之处欢迎大家指正，随着认识的加深这篇文章（包括设计模式其他系列文章）会不断地进行更新。

### javascript 设计模式

[面向对象的JavaScript](https://github.com/lvzhenbang/article/blob/master/design-pattern/oop-js.md)
	
[构造器 设计模式](https://github.com/lvzhenbang/article/blob/master/design-pattern/constructor.md)

[外观 设计模式](https://github.com/lvzhenbang/article/blob/master/design-pattern/facade.md)

[工厂 设计模式](https://github.com/lvzhenbang/article/blob/master/design-pattern/factory.md)

[观察者 设计模式](https://github.com/lvzhenbang/article/blob/master/design-pattern/observer.md)

[javascript 单例设计模式](https://github.com/lvzhenbang/article/blob/master/design-pattern/js-singleton.md)

[javascript 策略设计模式](https://github.com/lvzhenbang/article/blob/master/design-pattern/strategy.md)

[javascript 代理设计模式](https://github.com/lvzhenbang/article/blob/master/design-pattern/proxy-pattern.md)

[javascript 迭代器设计模式](https://github.com/lvzhenbang/article/blob/master/design-pattern/iterator.md)

[javascript 观察者模式](https://github.com/lvzhenbang/article/blob/master/design-pattern/js-observer.md)


持续更新中...

### 参考资料

[学习JavaScript设计模式](https://addyosmani.com/resources/essentialjsdesignpatterns/book/)

[腾讯全端 AlloyTeam 设计模式系列文章](http://www.alloyteam.com/2012/10/common-javascript-design-patterns/)

