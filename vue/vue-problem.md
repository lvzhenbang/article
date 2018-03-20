## vue 开发中的常见问题及注意事项

在vue的开发中我们常常会遇到这样或那样的问题，出现问题的大部分原因是我们代码书写不规范造成的。

### data

1.控制台提示“App.vue?ea99:28 Uncaught TypeError: Cannot read property 'splice' of undefined
    at VueComponent.sp (App.vue?ea99:28)”

解决方案：一个是出现了在data选项中未声明却被引用的变量；另一个就是低级错误(声明了变量，却引用错误)

