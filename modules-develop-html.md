# 前端开发——模块化（html模块化开发）
web从进入2.0时代后，web开发人员更加注重模块化思想的运用，特别是有了SPA之后。

> SPA——组件化
    
   进入了spa时代的我们对于模块化有了新的称呼‘组件化’，spa既是我们所熟知的单页面应用。

> spa 框架
        
    1.vue.js（推荐1）
    2.angularjs（推荐2）
    3.reactjs（推荐3）
    4.ember.js
    5.Aurelia.js
    6.backbone.js
    
> html组件化开发（推荐使用）vue.js

   随着各大浏览器运营商对es6的支持力度的加深，MVVM和组件化开发模式的流行，而目前最友好的支持他们的是vue.js和angular.js,出于个人感情我推荐vue.js,因为这个框架是国人开发的。
   
#### vue.js的组件化
   组件（Component）是 Vue.js 最强大的功能之一。组件可以扩展 HTML 元素，封装可重用的代码。组件是自定义元素，Vue.js 的编译器为它添加了特殊的功能。在有些情况下，组件也可以是原生 HTML 元素的形式，以 is 特性扩展。
    
   1.创建和注册组件：
    
   用Vue.extend()创建一个组件构造器：
    
    var MyComponent = Vue.extend({
        template : '<div>这是一自定义组件!</div>'
    })
    
   用Vue.component(tag, constructor)注册这个组件构造器(这个注册是全局的)：
   
    //全局注册组件，tag 为 my-component
    Vue.component('my-component',  MyComponent)
     
   初始化根实例化组件：
   
    <div id="example">
        <my-component></my-component>
    </div>
     
   渲染结果：
    
    <div id="example">
        <div>这是一自定义组件!</div>
    </div>
   vuejs组成
    1.指令
    2.路由
    3.状态
    4.生命周期
    5.方法
    6.选项
    7.属性
   vue.js的详解请参考官网文档：（https://cn.vuejs.org/）https://cn.vuejs.org/
> angularjs组件化
    demo：
    http://blog.csdn.net/lemon_zhao/article/details/52370079
    webuiangular组件库:
    http://www.wisoft.com.cn:8120/WebUI4Angular/docs/index_demo.html
## 参考资料
    组件化开发前世今生
    https://my.oschina.net/huangcongcong/blog/546001
    vue.js
    https://juejin.im/user/580327ee0e3dd900570cf3ab（文档，这几篇文章讲的很详细）
    https://vuefe.cn/v2/guide/（文档）
    https://github.com/lvzhenbang/my-blog（案例）
    https://github.com/lvzhenbang/shopping（案例）
    https://juejin.im/post/58e83e42570c350057cbac4d（案例）
    https://segmentfault.com/a/1190000008370588（案例）
    angular.js
    http://www.imooc.com/course/list?c=angularjs（慕课网系列教程）