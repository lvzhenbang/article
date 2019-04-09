# lazyload

## lazyLoad 是什么？

它是判断网页中目标元素是否出现在浏览器窗口，从而执行像页面中添加指定内容的一种加载策略。

## 它解决了什么问题？

当使用者在浏览器客户端发出一个HTTP请求后，服务器会向请求的浏览器发送相应的内容，然而不使用lazyload的话，浏览器会按照自己的解析策略来逐步加载用来呈现整个web页面的资源，如果页面包含较大的图片，字体或者js等静态资源，往往会造成浏览器渲染页面的过程被阻塞，这样必然带来较差的用户体验。

如果使用lazyload，默认的情况下，浏览器只会加载首屏内容。

注：首屏指出现在浏览器窗口，而页面未滚动时，用户看到的web页面部分。

注：内容指看到的浏览器呈现的页面效果，所需要的静态资源。

## 用途

图片、视频、音频、HTML元素等。

## 参考文章

[延迟加载图像和视频](https://developers.google.com/web/fundamentals/performance/lazy-loading-guidance/images-and-video/) google地址需要翻墙。

[搬移地址](https://github.com/dwqs/blog/issues/74)

## 常用插件

* [jquery lazyload](https://github.com/tuupola/jquery_lazyload) 基于jquery的
* [lazyload](https://github.com/verlok/lazyload) 兼容了IntersectionObserver
* [vue-lazyload](https://github.com/hilongjw/vue-lazyload) 基于vue.js的，兼容了IntersectionObserver
* [w3c IntersectionObserver](https://github.com/w3c/IntersectionObserver) polyfill
* [polyfillService IntersectionObserver](https://github.com/Financial-Times/polyfill-service)




