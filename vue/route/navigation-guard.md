## vue-router之导航守卫

说实话，到现在我都不太明白导航守卫使得字面意思。但其实说实话我们也不需要知道，我们只需要知道使用导航守卫我们能做什么就可以了。

使用路由守卫可以让我们很方便的控制路由的状态，在导航进入前，导航所指示的页面内容的更新，出导航，我们都可以通过相应的api进行控制。详情参考[官方文档](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html#%E5%AE%8C%E6%95%B4%E7%9A%84%E5%AF%BC%E8%88%AA%E8%A7%A3%E6%9E%90%E6%B5%81%E7%A8%8B)

在应用中出现路由跳转需要做一些验证，比如说登陆验证（在访问某个页面之前要看用户是否登录，如果未登录，就跳转到登录页面）。这个例子还需要使用[`路由元信息`](https://router.vuejs.org/zh/guide/advanced/meta.html)

参考资料：

[Learn Vue Router Navigation Guards Quickly](https://medium.com/vue-by-example/learn-vue-router-navigation-guards-quickly-1eb974097b85) **内置视频**
