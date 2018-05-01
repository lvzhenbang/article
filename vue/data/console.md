## vue 控制台提示的错误

1.控制台提示“App.vue?ea99:28 Uncaught TypeError: Cannot read property 'splice' of undefined
    at VueComponent.sp (App.vue?ea99:28)”

解决方案：一个是出现了在data选项中未声明却被引用的变量；另一个就是低级错误(声明了变量，却引用错误)