## 深入理解vue之组件

熟悉vue或者研究过vue源码的同学都知道，组件是vue最重要的部分之一，而写组件由两种常见的方式：

* template模板
* render渲染函数式的

### template式的组件

template式的组件有两种常见的形式：

第一种：

```
<template>
	<div class="hello">
		{{ msg }}
	</div>
</template>

<script>
Vue.component('hello', {
	data() {
		return {
			msg: 'hello world.'
		}
	}
})
</script>
```

第二种：

```
Vue.component('hello', {
	data() {
		return {
			msg: 'hello world.'
		}
	},
	template: `<div class="hello"> {{ msg }} </div>`
})
```

在项目开发中，第一种比较常见。

### render渲染函数组件

同样，vue本身也提供了性能更高的render函数渲染的组件。

```
Vue.component('hello', {
	data() {
		return {
			msg: 'hello world.'
		}
	},
	render: function(el) {
		return el(
			'div',
			this.msg
		)
	}
})
```

关于render的用法，可以参考vue.js官方文档介绍。[地址](https://cn.vuejs.org/v2/guide/render-function.html)

关于render的示例，可以参考bootstrap-vue的源码，它的组件很多都是使用render的。[GitHub地址](https://github.com/bootstrap-vue/bootstrap-vue)

### render函数渲染 VS template模板

* 后者适合逻辑简单，前者适合复杂逻辑。
* 后者属于声明是渲染，前者属于自定Render函数。声明式渲染，使用者理解起来相对容易，但灵活性不足；自定义render函数灵活性高，但对使用者要求较高。
* 前者的性能较高，后者性能较低。这一点我们可以看一下，下图中vue组件渲染的流程图可知。
* 基于上一点，我们通过vue组件渲染流程图知道，使用render函数渲染没有编译过程，相当于使用者直接将代码给程序。所以，使用它对使用者要求高，且易出现错误。

![vue组件渲染的流程图](https://github.com/lvzhenbang/article/blob/master/vue/img/render.png)

### 特殊组件

在vue中有一个特殊的组件，示例代码如下：

```
var vm = new Vue({
	data() {
		return {
			msg: 'hello world.'
		}
	},
	el: '#app'
})
```

这个特殊的组件指定我们使用vue的容器，并初始化一些数据。

### 虚拟DOM

虚拟DOM是vue.js的核心概念之一，vue.js使用js模拟DOM原型树，这就是我们常挂在嘴边的VNode。

vue.js发现模板的数据发生改变（vue是数据驱动的框架，这也是MVVM与传统的DOM结构驱动的区别），将会生成新的虚拟DOM，vue.js通过Diff算法来对比跟原来的Vnode是否相同来决定是否渲染该虚拟DOM，不相同则渲染。同时，vue2.x支持服务端的渲染。

