## eslint 使用总结

1. 在操作DOM对象的时候会报`no-params-reassign`这样的错误提示？

这个问题分为两种情况：

* 第一种是为dom添加它`prototype`上没有的属性时，

解决方案：可参考[7-12](https://github.com/airbnb/javascript#functions--mutate-params)这里的示例提示改进。

demo：[代码](https://github.com/lvzhenbang/webpack4.x-angularjs/blob/v3.4/src/directives/backtop.directive.js#L11)

* 第二种是操作版本较低的dom API，如: `element.style.display`

解决方案：可参考[ How to deal with DOM manipulation #766 ](https://github.com/airbnb/javascript/issues/766#issuecomment-189363766)。

demo: [代码](https://github.com/lvzhenbang/webpack4.x-angularjs/blob/v3.4/src/components/app/newscenter.component.js#L23)