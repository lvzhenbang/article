# 图片处理

注：在web开发中，小的图标图片建议使用`图标字体`，大图片也需要经过处理，然后再使用。

## 图片使用方式

* 默认带黑色边框
* 使用占位符
* 使用背景颜色
* 渐进式的渲染图片（progressive render picture）
* lazyload
* 或者占位符+lazyload，或者背景颜色+lazyload（后者骨架屏技术）

## 图片处理

### 雪碧图

* [` postcss-sprites `](https://github.com/2createStudio/postcss-sprites) 基于[` postcss `](https://github.com/postcss/postcss)的使用
* [` webpack-spritesmith `](https://github.com/mixtur/webpack-spritesmith) 是一个[` webpack `](https://github.com/webpack/webpack)插件

### 图片压缩

* [` imagemin `](https://github.com/imagemin/imagemin) 开源项目
* [` imagemin-jpegtran `](https://github.com/imagemin/imagemin-jpegtran#readme) jpg/jpeg
* [` imagemin-pngquant `](https://github.com/imagemin/imagemin-pngquant#readme) png
* [` imagemin-optipng `](https://github.com/imagemin/imagemin-optipng#readme) png 优化处理，可单独使用
* [` imagemin-svgo `](https://github.com/imagemin/imagemin-svgo#readme) svg
* [` imagemin-gifsice `](https://www.npmjs.com/package/imagemin-gifsicle) gif
* [` imagemin-webp `](https://github.com/imagemin/imagemin-webp#readme) webp

### 图片编辑器

* [` tui.image-editor `](https://github.com/nhn/tui.image-editor.git)

## html 到图片

* [` html2canvas `](https://github.com/niklasvh/html2canvas)
* [` dom-to-image `](https://github.com/tsayen/dom-to-image)
* [` web-file `](https://github.com/lvzhenbang/web-file)
