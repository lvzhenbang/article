## Vue 学习策略

学习Vue，使用Vue，研究Vue，然后成为一个Vue方面的专家并不是一件困难的事情。

随着vue的生态系统变得越来越大，以及vue版本自身的迭代，vue不再是那个人们认为的学习曲线平滑的框架。其实vue还是那样入门容易，上手简单，学习曲线平滑的工具，只不过是针对不同的开发情况，它又增加了一些辅助的工具库，来完善vue的开发解决方案。

如果我们只是要快速上手做一个规模并不是那么大的项目，只是用vue.js的核心库即可。从下面几个方面可以说明：

* 使用他提过的`component`选项或者`Vue.component()`API，我们可以定义并注册不同的视图；
* 使用它提供的`data`选项，我们可以统一管理每个视图的数据；
* 使用他提供的`filter`，`computed`或`methods`，我们可以对页面的state进行控制（通过处理`data`选项中的数据）；
* 通过双向数据绑定来实现数据在视图和模型之间的转换，利用`props`和`$emit`来实现不同组件（试图）间数据的传递；
* `methods` 除了可以控制数据外，它还是跟踪记录用户行为的重要途经。

通过上面的内容我们就可以快速的做一个简单易用，便于维护的应用。

当然，如果要实现对vue示例的细粒度控制，生命周期钩子是需要你深入理解的。

为了实现组件化开发，同时伴随着项目规模的增大，我们需要对vue的路由进行细粒度化的管理和vue的state做中心化管理。对于路由，我们使用 [`vue-router`](https://router.vuejs.org/)；而对于state，我们使用[`vuex`](https://vuex.vuejs.org/)。

单页面应用的缺点就是SEO，而[`vue-ssr`](https://ssr.vuejs.org/)就是这样一个解决方案，当然你也可以使用[`nuxtjs`](https://nuxtjs.org)这个集成vue.js的框架。使用SSR除了可以解决SEO的问题，还可以优化性能。

为了加快开发的速度，我们还可以使用关于vue前端模板，如：桌面版[Element](https://element.eleme.io/)，移动版[mintui](http://mint-ui.github.io/#!/en)，[iview](https://www.iviewui.com/)，以及[ant-design-vue](https://github.com/vueComponent/ant-design-vue)等。

再进一步就是`webpack+vue`，因为webpack这个构建工具不仅可以对项目做很多的性能优化提升，还可以是项目变得更加容易控制，管理与维护。
