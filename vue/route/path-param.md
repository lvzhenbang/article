## 由路径参数引起的路由问题

之前没有熟读vue-router开发文档之前，我总是遇到这样的问题，当路由参数改变时，路由从 `movie/26787574` 到 `movie/26787574` ，页面的数据没有刷新（注意：有一个前提就是就是路由变化前后两个路由公用一个组件），随后调试发现只有强制浏览器刷新，之后页面的数据才会跟着刷新。后来，仔细阅读文档发现，vue-router的内部有这样一个机制，就是当两个路由渲染同一个组件，比起销毁再创建，复用则显得更加高效，也就是说组件的声明周期钩子不会再被调用。

也就是说，要实现路由从 `movie/26787574` 到 `movie/26787574` 后页面数据的刷新，除了手动强制刷新这种很娄的办法外，我们可以尝试下面几种方法解决。

### watch选项监听 $route

参考示例的代码如下：

```
export default {
  name: 'HelloWorld',
  created: function() {
    this.getMovie()
  },
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      id: '',
      movie: { name: '', director: ''}
    }
  },
  watch: {
    '$route': "getMovie"
  },
  methods: {
    getMovie: function() {
      let id = this.$route.params.id
      this.$http.get('/api/' + id)
        .then(response => {
          console.log(this.movie, response.data)
          this.movie.name = response.data.title
          this.movie.director = response.data.author[0].name
        })
    }
  }
}
```

如果使用这种方式的话，会立即渲染导航和组件，然后公用组件的created钩子将获取数据。使用这种发式，我们可以在完成数据加载之前（特别是获取的数据比较大时），添加一个loading状态。

这种方式是在路由切换完成之后获取数据。

### beforeRouteUpdate()钩子

在2.2版本新增了一个beforeRouteUpdate钩子，它可以在导航完成前先获取数据，也即是说在说句获取成功后在执行路由切换

参考示例的代码如下：

```
export default {
  name: 'HelloWorld',
  created: function() {
    this.getMovie()
  },
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      id: '',
      movie: { name: '', director: ''}
    }
  },
  beforeRouteUpdate(to, from, next) {
    setTimeout(() => {
      this.getMovie()
      next()
    }, 3000)
  },
  methods: {
    getMovie: function() {
      let id = this.$route.params.id
      this.$http.get('/api/' + id)
        .then(response => {
          console.log(this.movie, response.data)
          this.movie.name = response.data.title
          this.movie.director = response.data.author[0].name
        })
    }
  }
}
```

### 通过特殊属性 `key`

在vue的虚拟DOM算法中，如果不使用 `key` 的vue会最大限度的减少动态元素并且尽可能的复用/修复相同类型元素。所以说，保持 `key` 的唯一性，就可以重新的渲染dom元素，因此在 `<router-view />` 这个渲染组件的容器内添加一个 `key` ，然后设定它的值为唯一即可。

```
<router-view :key="$route.path + new Date()" />
```
