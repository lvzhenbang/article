## JS开发中函数知识点梳理

要想学好JavaScript除了基本的JavaScript知识点外，作为JavaScript的第一等公民——函数，我们要深入的了解。函数的多变来源于参数的灵活多变的使用。如果参数是一般的数据类型或一般对象，这样的函数就是普通函数；如果函数的参数是函数，这就是我们所要知道的高级函数；如果创建的函数调用另外一部分（变量和参数已经预置），这样的函数就是偏函数。

此外，还有一点就是可选参数（optional parameter）的使用。

### 函数的分类

1. 普通函数

有函数名，参数，返回值，同名覆盖。示例代码如下：

```
function add(a, b) {
    return a + b;
}
```

2. 匿名函数

没有函数名，可以把函数赋值给变量和函数，或者作为回调函数使用。非常特殊的就是立即执行函数和闭包。

立即执行函数示例代码如下：

```
(function(){
    console.log(1)
})()
```

闭包示例代码如下：

```
var func = (function() {
    var i = 1;
    return function() {
        console.log(i);
    }
})()
```

3. 高级函数

高级函数就是可以把函数作为参数和返回值的函数。如上面的闭包。ECMAScript中也提供大量的高级函数如forEach(), every(), some(), reduce()等等。

4. 偏函数

```
function isType(type) {
    return function(obj) {
        return toString.call(obj) === "[object " + type + "]"
    }
}

var isString = isType('String');
var isFunction = isType('Function');
```

相信，研究过vue.js等常见库源码的同学不会陌生吧。

5. 箭头函数

箭头函数不绑定自己的this，arguments，super。所以它不适合做方法函数，构造函数，也不适合用call,apply改变this。但它的特点就是更短，和解决匿名函数中this指向全局作用域的问题

```
window.name = 'window';
var robot = {
    name: 'qq',
    print: function() {
        setTimeout(function() {
            console.log(this.name);
        }, 300)
    } 
};
// 修改1，用bind修改this指向
var robot = {
    name: 'qq',
    print: function() {
        setTimeout(function() {
            console.log(this.name);
        }.bind(this), 300)
    } 
};
// 修改2，使用箭头函数
var robot = {
    name: 'qq',
    print: function() {
        setTimeout(() => {
            console.log(this.name);
        }, 300)
    } 
};
```

想了解更多箭头函数可以看[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

### 函数的参数

1. 传入明确的参数
```
function add(a, b) {

    reutrn a + b;
}
```

2. 使用arguments对象

```
function add() {
    var argv = Array.prototype.slice.apply(arguments);
    return argv.length > 0 ? argv.reduce(function(acc, v) { return acc+=v}): '';
}
```

3. 省略参数，参数默认值

```
function sub(a, b) {
    a = a || 0;
    b = b || 0;
    return a - b;
}
```

4. 对象参数

```
var option = {
    width: 10,
    height: 10
}

function area(opt) {
    this.width = opt.width || 1;
    this.height = opt.height || 1;
    return this.width * this.height
}
```

对象参数比较常见，常出现在jQuery插件，vue插件等中。

5. 可选参数

ES5实现可选参数，我们需要使用arguments。使用指定范围的可选参数我们一般使用发对象参数，写过jQuery等插件的应该印象深刻。

6. ES6中的函数参数

在ES6中，参数默认值，省略参数操作使用比较简便。示例代码如下：

```
var area = (width=1, height=1) => width*height
```

在ES6中，使用可选参数。示例代码如下：

```
var add = (...nums) => {
    var numArr = [].concat(nums)
    return numArr.reduce((acc, v) => acc += v)
}
```

7. 解构参数

```
myFunc = function({x = 5,y = 8,z = 13} = {x:1,y:2,z:3}) {
    console.log(x,y,z);
};

myFunc(); //1 2 3  (默认值为对象字面量)
myFunc({}); //5 8 13   (默认值为对象本身)
```

### 函数的返回值

1. 函数的返回值为基本数据类型，如字符串，数字，Boolean，null，undefined。示例代码如下：

```
function add(a, b) {
    return a + b
}
```

2. 函数的返回值为对象。示例代码如下：

```
function Robot(name) {
    this.name = name
}

Robot.prototype.init = function() {
    return {
        say: function () {
            console.log('My name is ' + this.name)
        }.bind(this),
        dance:  function(danceName) {
            console.log('My dance name is ' + danceName)
        }
    };
}

var robotA = new Robot('A');
robotA.init().say(); // "My name is A"
var robotB = new Robot('B');
robotB.init().say(); // "My name is B"
```

不管是写原生还是jQuery插件，亦或其他插件，这种情况都不少见。更深入的了解可以参考jQuery源码。

3. 返回值为函数

这个我们最为熟悉的莫过于闭包。具体可参考
[老生常谈之闭包](https://github.com/lvzhenbang/article/blob/master/js/closure.md)

### 参考文章

[JS: How can you accept optional parameters?](http://lucybain.com/blog/2015/js-optional-parameters/)

[Named and Optional Arguments in JavaScript](https://medium.com/dailyjs/named-and-optional-arguments-in-javascript-using-es6-destructuring-292a683d5b4e)

[How to use optional arguments in functions (with optional callback)](http://www.jstips.co/en/javascript/use-optional-arguments/)

[JavaScript Functions that Return Functions](https://davidwalsh.name/javascript-functions)