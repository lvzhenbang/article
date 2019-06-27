# css

css看起来简单，但是高手和初学者写出来的结果有很大的差别。

css高手具备以下特点？

* 不仅懂得css的`基本定义`，
* 理解css的[`工作流`](https://vimeo.com/15982903)（workflow）,
* 更懂得如何使用`工具`减轻自己的工作量，
* 同时更注重css的模块化（通过@import来连接不同的模块）和语义化（BEM）

## 概念与定义

* [CSS Working Group Specifications](https://github.com/w3c/csswg-drafts) github项目
* [css animation](https://github.com/w3c/css-houdini-drafts.git)

## 预处理工具

* [` sass `](https://github.com/sass/sass)
* [` less `](https://github.com/less/less.js)
* [` stylus `](https://github.com/stylus/stylus)

注：可以使用它们的脚本命令，也可以使用构建工具，如：webpack，可以参考[` webpack-play `](https://github.com/lvzhenbang/webpack-play)

## css3 animate

* [aniamte.css](https://github.com/daneden/animate.css)
* [loader.css](https://github.com/ConnorAtherton/loaders.css)
* [epic-spinners](https://github.com/epicmaxco/epic-spinners)
* [hamburgers](https://github.com/jonsuh/hamburgers)
* [SpinKit](https://github.com/tobiasahlin/SpinKit)

## css语法检测

* [` stylint `](https://github.com/stylelint/stylelint)

### 预配置语法规则

* [` stylint-config-standard `](https://github.com/stylelint/stylelint)
* [` css `](https://github.com/airbnb/css) 来自` airbnb `

### autoprefixer

* [`atuoprefixer`](https://github.com/postcss/autoprefixer) 为不同的浏览器，增加相应的浏览器前缀

### css压缩

* [` cssnano `](https://github.com/cssnano/cssnano) 一个`postcss`插件
* [` optimize-css-assets-webpack-plugin `](https://www.npmjs.com/package/optimize-css-assets-webpack-plugin) 一个`webpack`插件

### 构建工具（webapck）

* [` extract-text-webpack-plugin `](https://github.com/webpack-contrib/extract-text-webpack-plugin) 把`css`打包到一个文件中。webpack版本小于4.x
* [` mini-css-extract-plugin `](https://github.com/webpack-contrib/mini-css-extract-plugin) 同上，webpack4.x以及之后的版本
* [` purifycss-webpack `](https://github.com/webpack-contrib/purifycss-webpack) 移除未使用到的css样式

## css模块化

* [` BEM `](https://en.bem.info/methodology/quick-start/) 文档，[` bem `](https://github.com/bem)GITHUB开源项目
* [` SMACSS `](https://clubmate.fi/oocss-acss-bem-smacss-what-are-they-what-should-i-use/)

注：可参考[` inuit.css `](https://github.com/csswizardry/inuit.css) 项目，或者[` sass-smacss `](https://github.com/jonathanpath/SASS-SMACSS)

## 参考资料

* [` You-need-to-know-css `](https://github.com/l-hammer/You-need-to-know-css)
* [` You-Dont-Need-JavaScript `](https://github.com/you-dont-need/You-Dont-Need-JavaScript)
* [` materialize `](https://github.com/Dogfalo/materialize) a CSS Framework
