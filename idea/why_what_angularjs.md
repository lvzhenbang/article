# angularjs

angularjs的学习曲线并没有想象中的那么难学和使用，只要理解其中关键概念的定义，学习和使用起来就简单得多了。我第一次接触angularjs，因为不理解它其中的概念，所以使用时非常困惑，只知道他就是该这么用，而不知道如何灵活的运用它。像其他的框架一样，angularjs其实上手也特别快，但是想要驾轻就熟有点儿难，现在找到了问题，下面就详细说说。

## 什么是MVC

它是Model，View，Controller的简称

Model：它定义了应用中的数据；
View：它包含了应用要展示的html代码块；
Controller：它将Model中的数据绑定到了View的html代码块中。通过Controller的可以实现这样的功能：当Model中的数据改变View的相应部分也会改变；View中的代表数据的部分发生改变，Model中相应的数据也会改变。

## 什么是MV*

只关注Model和View，而不管angularjs是什么。angularjs会将你的model数据（JavaScript的Object，变量）绑定到视图（HTML或DOM）中，而事实上angularjs确实符合这样的特点。

## 什么是MVVM

它是Model，View，ViewModel的简称。

Model和View的定义和上面MVC中介绍的类似。

ViewModel：它和MVC中的Controller的作用类似，但是又有所不同，下面将做详细介绍。

## MVC vs MVVM

我在[stack overflow](https://stackoverflow.com/questions/39523713/why-is-angular-called-mv-framework)s上看到一个对这个问题的描述。

![mvc vs mvvm](https://i.stack.imgur.com/qMNP8.png)

MVC的场景更像是这样：

* 你进入一家餐馆，为你服务的是Controller，他接受你input的订单；
* Controller将订单，交给了大厨（Model），大厨（Model）做完后，叫了（View）去送餐；
* 然后送餐员（View）将餐品送给你手中。

MVVM的场景是这样的：

* 你进入一家餐馆，为你服务的是View，他接受你input的订；
* 将订单交给ViewModel，而由ViewModel将订单交给厨师（Model），大厨（Model）做完后将餐品交给ViewModel，然后由ViewModel将餐品交给View；
* 最后View将餐品交到你手中。

所以，通过比较你会发现MVVM设计的更合理，效率更高。更准确的来说MVC来自于SERVER，后端的开发，而MVVM则是真正的前端设计。在angularjs中Controller其实是充当的ViewModel的作用，我们在开发的时候一般会用到vm，或见过一些大神用vm，其实vm就是ViewModel的引用。

## 如何使用model

angularjs提供了Factories，Services，providers或者REST Services + $http，通过它们可以方便的操作数据。

## 什么是依赖注入

我先介绍一个常用的开发业务，代码如下：

```
var Person = function(name, age) {
  this.name = name
  this.age = age
}

function logPerson() {
  var p1 = new Person ('马龙', 26)
  console.log(p1)
}

logPerson()
```

在上面的代码中，先定义了一个构造函数Person，然后定义了一个输入Person实例的函数。当然，这种写法不好，如果要输出100个不同的Person实例，就要定义100个类似的函数，我们可以修改代码如下：

```
...
function logPerson(person) {
  console.log(person)
}

var p1 = new Person ('马龙', 26)
logPerson(p1)
```

将Person的实例化提出来，logPerson只做Person的实例输出，输出的具体实例依赖于传入的参数，而具体的实例化过程则与它无关，这样做符合了开发中的单一只能原则。与此同时在某种程度上解释了angularjs的依赖注入。是不是感觉不可思议，说实话就是这么简单。

```
var myApp = angular.module('myApp', [])

myApp.controller('mainController', function($scope) {
  $scope.name = 'jane Doe'
  console.log($scope)
  $scope.getname = function() {
    return 'jane Doe'
  }
})
```

如上面我们常见的代码，其中`[]`中就是`myApp`依赖的模块，而控制器定义的名字为`mainController`的控制中的回调函数中传入的`$scope`参数也是一个依赖，控制器`mainController`需要用`$scope`的来实现将Model中的数据绑定到View中。

我们可以使用Factories，Services定义依赖，在创建的controller中来使用，代码如下：

```
var myApp = angular.module('myApp', [])

myApp.factory('greeter', function($window) {
  return {
    greet: function(text) {
      $window.alert(text)
    }
  }
})

myApp.controller('mainController', function($scope, greeter) {
  $scope.sayHello = function() {
    greeter.greet('hello Angularjs.')
  }
})
```

[demo](https://codepen.io/lvzhenbang/pen/wQOrpw)

依赖注入的具体使用可参考[AngularJS 依赖注入](http://www.angularjs.net.cn/tutorial/17.html)

## 作用域（scope）

scope是angularjs中的一个重要概念，它负责将Model数据绑定到View中。它是angularjs内置的一个重要的依赖注入。如果你使用的多了你会发现其实scope的作用有点儿像是MVVM中的ViewModel。

## 指令（directive）

通过指令可以实现对DOM的操作。

angularjs中指令分为内置指令和自定义指令这里不做介绍，内置指令可参考[官方文档-内置指令](http://www.angularjs.net.cn/api/ng/directive)，自定义指令的具体使用可参考[官方文档-自定义指令](http://www.angularjs.net.cn/tutorial/5.html)

当然，这里只是讲述了如何快速的上手angularjs，angularjs还有其它的特性，如下图所示：

![angularjs 全部特性](https://ss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=2594402509,2287782140&fm=15&gp=0.jpg)


## 路由（routes）

视图（View）是angularjs的一个重要概念，每个视图基本是由指令和控制器来组成。而一个应用有不同的视图构成，所以路由这个概念应运而生。

具体使用可参考[ng-routes](http://www.runoob.com/angularjs/angularjs-routing.html)。（这里吐槽一句，中文文档做的有点儿差劲。）



具体使用，需要你查阅[官方文档](http://www.angularjs.net.cn/tutorial/)来学习。