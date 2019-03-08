## eslint

使用`webpack`构建项目的话，需要引入 [`eslint-loader`(https://www.npmjs.com/package/eslint-loader)，通过这个loader可以来调用用 [`eslint`](https://www.npmjs.com/package/eslint)。

在webpack进行构建输出前，`eslint` 它起到检查源文件代码的语法是否符合规范的作用。

`eslint`随着这几年的发展，它的规模越来越强大，同样它支持的语法规则检查也越来越多，如果刚上手就尝试自己配置语法规则，可能会比较困难，相对来说使用第三方的扩展会轻松许多，它们的名字格式都是`eslint-config-*`，最后一个破折号（英文的）后跟配置的名字，如：`eslint-config-airbnb`，`eslint-config-standard`等。

注：个人比较喜欢`eslint-config-airbnb`。

### webpack中引入 eslint

首先，引入`eslint-loader`，以及 `eslint` 安装命令如下：

```
yarn add eslint eslint-loader --dev
```

然后，在`webpack.config.js`中，使用`eslint-loader`，为源文件开启`eslint`功能，配置如下：

```
module: {
  rules: [
    {
      enforce: 'pre',
      test: /\.js$\/,
      exclude: /(node_modules)/,
      loader: 'eslint-loader'
    }
  ]
}
```

注：`enforce`的作用是强制 `eslint-loader` 最先运行，避免如 `babel-loader` 先编译源文件的问题，而造成无法对源文件的语法进行检测的情况。

然后，安装 `eslint-config-airbnb` ，命令如下：

```
yarn add eslint-config-airbnb --dev
```

紧接着，在在项目的根目录中添加一个`.eslintrc`的文件（其它选择，可参考[ `配置文件形式` ](https://eslint.org/docs/user-guide/configuring#configuration-file-formats)），配置如下选项：

```
{
  "extends": 'airbnb'
}
```

这样运行`npm run dev`或者`npm run build`就可以使用`eslint`对项目进行`javascript`相关的语法检测。如果项目源文件的语法格式与`eslint-config-airbnb`中定义的规则相冲突，构建输出就会提示相关的错误信息。

注：`airbnb` 定义的相关语法规则可参考[ `文档` ](https://github.com/airbnb/javascript#airbnb-javascript-style-guide-)。

### 参考资料

[eslint 使用总结](https://github.com/lvzhenbang/article/blob/master/lint/eslint-doc.md)