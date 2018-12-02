# promise

Promise 就是用来做异步处理的，最早是浏览器的DOM实现了它，后来JavaScript也实现了它。但有时在业务开发的时候，我们既要使用异步的优势，又要使用异步处理的结果（也就是某些操作要依赖异步处理的结果）。在没有Promise之前，我们使用嵌套函数结合return来使用，当然模拟异步我们使用setTimeout。这也就是人们常说的 `异步-等待` 。

## async-await

最新版的ES7定义了`async` 和 `await` ，它可以让我们轻松的实现异步（用async），而使用异步结果可以使用await，这是目前公认的最佳的解决方案，当兼容性欠佳。

包括现在最流行的python框架[vibora](https://github.com/vibora-io/vibora) 也实现了类似的语法结构，这是我比较喜欢的框架，比起dijango来说。

它们使用前来很简单，可参考[MDN (async-await)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)

在这之前的ES6实现了Promise。这个也是目前使用最广泛。

## Promise

Promise是一种链式使用的解决方案，如果经常使用jQuery的话，学习Promise很快。但这里我还是想说一下，Promise有一个关键点，第一个就是单独将一个代码块实现异步，可以使用Promise构造函数来实现，异步处理的结果在构造函数中使用resolve和reject来处理，它们前一个代表成功，后一个代表失败，成功的结果我们可以用一个或多个then来一步一步的做处理，当然我们要对异常进行处理的话，那么就必须使用catch，但是它捕获的是reject返回的结果。当然还有一些其它api如race，它只处理多个异步中最先完成的那个。finally即处理resolve，也处理reject。all是对多个业务模块进行异步处理。

这里说的不仅详细，你可以参考[promise cookbook](https://github.com/mattdesl/promise-cookbook/blob/master/README.md)，你也可以参考[The Promise of a Burger Party](https://kosamari.com/notes/the-promise-of-a-burger-party)，你还可以参考[Jake Archibald 写的Promise](https://developers.google.com/web/fundamentals/primers/promises)。

Promise其实也没有什么，多看几位大牛写的文章，使用中的问题就迎刃而解，以上的几篇文章，从多角度方面来讲Promise。

然而，在这之前，我们使用最多的是jQuery，而jQuery介绍的defferred。

## 其它实现

[Q](https://github.com/kriskowal/q)

[defferred](https://api.jquery.com/category/deferred-object/)

[when](https://github.com/cujojs/when)

[WinJs](https://docs.microsoft.com/en-us/previous-versions/windows/apps/br211867(v=win.10))

## 如何用Promise处理defferred

JavaScript promise API 将任何使用 then() 方法的结构都当作 promise 一样（或按 promise 的说法为 thenable）来处理，因此，如果你使用返回 Q promise 的库也没问题，因为它能与新 JavaScript promise 很好地兼容。

如我之前所提到的，jQuery 的 Deferred 不那么有用。幸运的是，你可以将其转为标准 promise，这值得尽快去做：

```
var jsPromise = Promise.resolve($.ajax('/whatever.json'))
```

这里，jQuery 的 $.ajax 返回了一个 Deferred。由于它使用 then() 方法，因此 Promise.resolve() 可将其转为 JavaScript promise。但是，有时 deferred 会将多个参数传递给其回调，例如：

```
var jqDeferred = $.ajax('/whatever.json');

jqDeferred.then(function(response, statusText, xhrObj) {
  // ...
}, function(xhrObj, textStatus, err) {
  // ...
})
```

而 JS promise 会忽略除第一个之外的所有参数：

```
jsPromise.then(function(response) {
  // ...
}, function(xhrObj) {
  // ...
})
```
幸好，通常这就是你想要的，或者至少为你提供了方法让你获得所想要的。另请注意，jQuery 不遵循将 Error 对象传递到 reject 这一惯例。


文章到这里，基本就结束了。很多人会问，这篇文章中的内容别的文章都介绍过，为什么还要介绍，这篇文章的目的就是将Promise的相关知识点都提一下，方便对Promise进行专项的研究和分析。

## 更深层的学习

[Promise的前世今生](http://www.html5rocks.com/en/tutorials/es6/promises/)

[Promise存在的问题](http://pouchdb.com/2015/05/18/we-have-a-problem-with-promises.html)

[Promise的要点分析](https://blog.domenic.me/youre-missing-the-point-of-promises/)