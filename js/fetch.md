## $.ajax vs axios vs fetch


* `Ajax` 是什么？

答：Ajax是一种可以在浏览器和服务器之间使用异步数据传输（HTTP请求）的技术。使用它可以让页面请求少量的数据，而不用刷新整个页面。而传统的页面（不使用Ajax）要刷新部分内容，必须重载整个网页页面。

* `Ajax` 基于什么？

答：它基于的是XMLHttpRequest（XHR）。这是一个比较粗糙的API，不符合关注分离的设计原则（Separation of Concerns），配置和使用都不是那么友好。

* `$.ajax` 的产生背景？

答：基于上面的原因，各种ajax库引用而生，然而最有名的就是jQuery的API中的 `$.ajax()` 。 `$.ajax()` 它的一个优势异步操作，但jQuery的异步操作是基于事件的异步模型，没有promise那么友好。

* `fetch` 产生的背景？

答：综合上面所讲的各种因素，fetch这个api应运而生。但好用归好用，它存在着一些[问题](https://medium.com/@thejasonfile/fetch-vs-axios-js-for-making-http-requests-2b261cdd3af5)（这个问题下面详讲，并讲解相对应的解决方案），再加上兼容性问题（ie压根不支持），所以很多开发者使用了axios这个第三方库。

* 支持promise的库（axios）？

答：`axios` 这个库现在是一个比较通用的行业解决方案，`axios` 流行开来的一个原因是promise，另一个原因是基于数据操作的库的流行（vue.js, angular.js, react.js等），而传统的jQuery是基于dom操作的库。但它也存在着缺陷，就是我们使用前，要保证这个库已经被引入。

其实，就我个人而言，我还是比较喜欢使用 `fetch` 的。在开发中遇到兼容性的问题，只需要同构fetch即可，而不需要额外的引入一个库。下面就重点说一下fetch。

### fetch 的使用

```
fetch(url, options)
    .then(response => console.log(responese))
    .catch(err => console.log(err))
```

url：访问地址
options：常用配置参数
response: 请求返回对象

请求参数配置 `options` 详情可参考[MDN fetch](https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API/Using_Fetch#%E6%94%AF%E6%8C%81%E7%9A%84%E8%AF%B7%E6%B1%82%E5%8F%82%E6%95%B0)

### fetch存在的问题及解决方案

1. 得到数据你需要两个步骤

```
fetch('https://api.github.com/users/lvzhenbang/repos')
    .then(res => {
        console.log(res)
        return res.text()
    }).then(data => {
        console.log(data)
    })
```

通过上面的代码，可以发现直接打印返回的 `Response` 对象中压根就没有数据，要想获取到所需的数据，必须经一个中间的方法 `response.text()` （fetch提供了5中[方法](https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API/Using_Fetch#Body)）

而反观 `axios` 使用起来就要方便的多，它返回的 `Response` 对象中却有数据，在 `data` 属性内。参考代码如下：

```
axios.get('https://api.github.com/users/lvzhenbang/repos')
    .then(res => console.log(res));

```

当然，这也不算是一个大问题，就是使用略显麻烦了点。

2. fetch的请求默认是不带 `cookie`

解决这个问题，需要在 `options` 中配置 `{credentials: 'include'}`

3. 并非所有的请求错误都会 `reject`

也就是说 `catch` 方法并不能捕获所有的错误，当错误可以用一个状态码（如：404，500等）的形式表示时，`fetch` 返回的 `Promise` 不会拥有reject，只有当网络故障或请求被阻止时 `catch` 才有效。

解决这个问题，我们可判断 `Response` 对象中的 `ok` 是否为true，如果不是，用 `Promise` 手动添加一个 `reject` 即可。参考代码如下：

```
fetch('https://api.github.com/usrs/lvzhenbang/repos')
    .then(res => {
        if (res.ok) {
            return res.text()
        } else {
            return Promise.reject('请求失败')
        }
    }).then(data => {
        console.log(data)
    }).catch(err => {
        console.log(err)
    })
```

如果不手动添加 `reject` 将打印出 `undefined`，而这并不是我们想要的，当然使用 `axios` 则不需要考虑这个问题，代码如下：

```
axios.get('https://api.github.com/usrs/lvzhenbang/repos')
    .then(res => console.log(res))
    .catch(err => console.log(err));
```

### fetch 的优化

由于 `res.text()` 方法返回的就是一个 `promise` ，所以可以直接调用 `.then` ；此外为了保证所有的错误都返回一个统一的格式（都返回一个 `Promise`），上面的代码可以优化如下：

```
fetch('https://api.github.com/usrs/lvzhenbang/repos')
    .then(res => {
        return res.text()
            .then(data => {
                if (res.ok) {
                    return data
                } elese {
                    return Promise.reject(json)
                }
            })
    }).then(data => {
        console.log(data)
    }).catch(err => {
        console.log(err)
    })
```

玩过express/koa的同学，或者对后端有一定了解的同学都知道，服务器在某些情况下也会返回一些提示信息，那么应该如何处理呢？常见的错误提示包括一个状态码（status）和提示信息（msg），代码修改如下：

server:
```
res.status(404).send({
    err: 'not found'
})
```

client:
```
fetch('https://api.github.com/usrs/lvzhenbang/repos')
    .then(handleResponse).then(data => {
        console.log(data)
    }).catch(err => {
        console.log(err)
    })

function handleResponse (res) { 
    return Promise.reject(Object.assign({}, res.text(), {
        status: res.status,
        msg: res.statusText
    }))
}
```

### 兼容解决方案

以上问题解决并优化fetch的使用后，发现fetch还是一个不错的选择。针对不同使用情况可以作如下处理：

首先，要引入 [`es5-shim`](https://github.com/es-shims/es5-shim) 解决fetch这个新特性的同构；

其次，要引入 [`es6-promise`](https://github.com/jakearchibald/es6-promise) 解决promise的兼容问题；

然后，引入 [`fetch-ie8`](https://github.com/camsong/fetch-ie8) 解决fech的同构问题；

最后，引入 [`fetch-jsonp`](https://github.com/camsong/fetch-jsonp) 解决跨域问题。

当然，你也不需要针对性的解决这些问题，GitHub团队提供了一个[polyfill解决方案](https://github.com/github/fetch)，你不需要一步步的是实现。只需要两步：

1. 安装 `fetch` package

    npm install whatwg-fetch --save

2. 在使用的模块引入 `fetch`

```
import 'whatwg-fetch'

window.fetch(url, options)
```

其他的使用和 `fetch` 则这个原生的API雷同。

### 哪些情况可以放弃使用fetch

1. 获取Promsie的状态，如：isRejected, isResolved

2. 如果使用习惯了jquery的progress方法的，或者使用deffered的一些方法

具体 `fetch` 实现了哪些与jquery类似的方法可参考[whatwg-ftch](https://github.com/whatwg/fetch/issues/27) 或者 [fetch-issue](https://github.com/whatwg/fetch/issues/27)