# vue-router

`Vue.js` 官方推荐我们使用[vue-router]这个库，但是对于一个十分简单的项目，我们也可以完全避免。

## 如果不用vue-router，我们该怎么办

可参考[vue-2.0-simple-routing-example](https://github.com/chrisvfritz/vue-2.0-simple-routing-example)。

当然，不仅限于vue官方提供的开发模式，使用`vue-cli`, 我们也可以根据自己公司的需求和项目的实际情况来修改代码，总之想怎么用就怎么用，不要被工具框住，我们也可以像下面这样用：

```html
<div id="app">
  <router-link href="/">Home</router-link>
  <router-link href="/about">about</router-link>
  <router-link href="/error">error</router-link>
  <router-view></router-view>
</div>
```

```javascript
<script src="https://unpkg.com/vue/dist/vue.js"></script>
<script>
// define page component
const Home = { template: '<p>home page</p>' }
const About = { template: '<p>about page</p>' }
const NotFound = { template: '<p>Page not found</p>' }
// route map
const routes = {
  '/': Home,
  '/about': About
}
// define route component
const RouterLink = Vue.component('RouterLink', {
  props: {
    href: {
      type:String,
      required: true 
    }
  },
  template: `<a
    :href="href"
    :class="{ active: isActive }"
    @click="go"
  >
  <slot></slot>
  </a>`,
  computed: {
    isActive () {
      return this.href === this.$root.currentRoute
    }
  },
  methods: {
    go (event) {
      event.preventDefault()
      this.$root.currentRoute = this.href
    }
  }
})
const RouterView = Vue.component('RouterView', {
  computed: {
    ViewComponent () {
      return routes[this.$root.currentRoute] || NotFound
    }
  },
  render (h) {
    return h(this.ViewComponent)
  }
})
// Vue instance
const app = new Vue({
  el: '#app',
  data: {
    currentRoute: '/'
  },
  component: {RouterLink, RouterView}
})
</script>
```

除此之外就是根据需要使用sea.js，require.js等将组件模块化，使用这种倒退的开发模式。这样做就是为了配合哪些习惯了使用sea.js等的人（因为我遇见了很多在比较多这样的情况）。

## 为什么使用vue-router

vue-router的好处就是可以对导航（navigation）做细粒度的控制，如：导航触发前的控制，视图更新前的控制等；也可以对视图（view）做一些视图控制，如：视图的数据的获取，视图的lazyload，transition等。

入门示例可参考[router 入门](https://router.vuejs.org/zh/guide/#html)

vue-router其实也没有多少东西，其中一个就是配置`routes`选项，就成功了一半，其实就是学好 `RouteConfig` ：

```
declare type RouteConfig = {
  path: string; // 导航（路由）的路径，类型是字符串
  component?: Component; // 组件（路由的视图），类型是组件
  name?: string; // for named routes // 路由的名字，类型是字符串
  components?: { [name: string]: Component }; // 就像vue实例中那样引入组件一样
  redirect?: string | Location | Function; // 重定向路由，类型是字符串，函数或自定义的location类型
  props?: boolean | Object | Function; // 向视图传递数据，可以是动态的，也可以是静态的。如果是布尔类型的且是true，那么默认传递的是路由参数，反之则不传递；如果是对象的话，就是自定义的静态数据；如果是函数的话，就是自定义的动态的数据。
  alias?: string | Array<string>; 路由的别名，类型可以是字符串，也可以是一个字符串数组
  children?: Array<RouteConfig>; // 定义子路由
  beforeEnter?: (to: Route, from: Route, next: Function) => void; // 定义路由内的导航守卫
  meta?: any; // 这个字段的使用，会让你加深对路由守卫的理解，

  // 2.6.0+
  caseSensitive?: boolean; // 匹配规则是否大小写敏感？(默认值：false)
  pathToRegexpOptions?: Object; // 编译正则的选项
}
```

其实，这个就是用TypeScript定义的一个`路由映射对象`（也叫做：路由记录）的类型，其中`**?: **`这样的是可选的属性，其中`path`是必须使用的。

当然你也可以使用[编程式路由](https://router.vuejs.org/zh/guide/essentials/navigation.html)

另一个就是导航的守卫，你需要理解导航解析流程：

1. 导航被触发。
2. 在失活的组件里调用离开守卫。
3. 调用全局的 beforeEach 守卫。
4. 在重用的组件里调用 beforeRouteUpdate 守卫 (2.2+)。
5. 在路由配置里调用 beforeEnter。
6. 解析异步路由组件。
7. 在被激活的组件里调用 beforeRouteEnter。
8. 调用全局的 beforeResolve 守卫 (2.5+)。
9. 导航被确认。
10. 调用全局的 afterEach 钩子。
11. 触发 DOM 更新。
12. 用创建好的实例调用 beforeRouteEnter 守卫中传给 `next` 的回调函数。

玩转了导航守卫，然后再加上其它一些知识点，vue-router就可以完全拿下。

## 用或不用取决于实际的需要

千万不要做这样的事情——‘为赋新词，强说愁’，好的开发人员要对自己的代码负责，对自己的工作尽职。