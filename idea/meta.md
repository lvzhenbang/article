# meta

meta中有这样几个常用属性：http-equiv，name，content，包括html5新增的charset。

注意：content属性用来存储meta信息的内容，所有的主流浏览器都支持它，但它一般很少单独使用，我们一般使用http-equiv或name来定义content属性信息（或值）的名称，http-equiv和name在一个meta中通常只能用其中一个。

name常见的有：

```
application-name // 代表web应用程序的名字
author // 规定文档作者的名字
description // 对页面的描述，SEO需要用到
keywords // 页面的关键字词，多个用逗号隔开，SEO需要用到
```

## 编码格式

```
<meta charset="utf-8">
```

## 视窗宽度

```
<meta name="viewport" content="width=device-width, inital-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no"/>
```
* width：用于设置视窗口的宽度，可以设置为device-width（设备的宽度），也可以是自定义的值。
* initial-scale：用于设置缩放比例，可以是任意数值的比例。
* minimum-scale：用于设置最小缩放比例。
* maximum-scale：用于设置最大缩放比例。
* user-scalable：用于设置是否禁止用户缩放页面。

## 自动识别

```
<meta name="format-detection" content="telphone=no">
```

* telphone：是否禁止浏览器识别页面中的电话号码。no：禁止，yes不禁止
* eamil：是否就职浏览器识别页面中的邮件地址。no：禁止，yes不禁止

# 用http-equiv于模拟一个 HTTP 响应头

我们都知道，HTML4.0.1定义html文档的编码方式是如下面这样：

```
<meta http-equiv="content-type" content="text/html; charset=UTF-8">
```

但是在HTML5中我们可以像下面这样定义：

```
<meta charset="utf-8">
```

那么它除了这样的使用场景外，还有别的一些吗。如果你强制刷新页面，可以驶入像下面这样的代码：

```
<meta http-equiv="refresh" content="3">
```

如上面的代码的意思是：强制浏览器每3秒刷新一次。但要慎用。

当然，还有一些其他的好用的效果，如果你想要给应用定义多套样式，然后根据用户选择来加载不同的样式，你可以将`http-equiv`设置为`default-style`，然后你设置content的值为link或style的对应值。具体可参考如下代码：

```
<meta http-equiv="default-style" content="s1">
<style title="s1">
body {
  background: red;
}
</style>
<style title="s2">
body {
  background: green;
}
</style>
```

以前，我不知道style或link中添加title有什么用，但上面的例子是它的一个用途。

## 针对苹果设备的设置

```
// 下面代码来自天猫移动web
<meta name="apple-mobile-web-app-capable" content="yes"/> // 可以让app运行于全屏模式
<meta name="apple-mobile-web-app-title" content="TMALL"/> // 可以让app的标题不同于页标题
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent"/> // 配置app的状态栏，可设置为default, black, 和 black-translucent。
```
状态栏设置，可参考[Changing The iOS Status Bar Of Your Progressive Web App](https://medium.com/appscope/changing-the-ios-status-bar-of-your-progressive-web-app-9fc8fbe8e6ab)

当然，我们也可以设置书签或者快捷键的图标。可参考如下代码：

```
<link href="https://placehold.it/72"
          sizes="72x72"
          rel="apple-touch-icon-precomposed">
```

可以根据不同的型号设备，来设置不同的sizes。

也可以设置它的启动背景图。可参考如下代码：

```
<link href="https://placehold.it/1496x2048"
          media="(device-width: 768px) and (device-height: 1024px)
                 and (-webkit-device-pixel-ratio: 2)
                 and (orientation: landscape)"
          rel="apple-touch-startup-image">
```

当然，我们也可以设置其它的meta，用来控制不同浏览器的行为，也可以控制不同搜索引擎的行为。

## 其他meta的使用

可参考[gethead meta](https://github.com/joshbuchea/HEAD#meta) 或 [模拟原生IOS效果](https://gist.github.com/tfausak/2222823)。