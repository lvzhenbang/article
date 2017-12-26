##浏览器渲染html文档的有几种模式，区别是什么

浏览器的渲染模式有3种,分别是怪癖模式（兼容模式）[quirks mode]、标准模式[standards mode]、几乎标准模式[almost standards mode]

一些常见的DOCTYPE定义在不同浏览器下所呈现的不同形式如下图所示：

[doctype-browser](https://github.com/lvzhenbang/article/tree/master/interview/img/dtd.png)

通过图表的数据可知 `<!DOCTYPE html>` IE>=8,以及其它主流浏览器在这中文档的定义规则下都会采用标准模式对html页面进行渲染。


### 怪癖模式

当页面上没有 `<!DOCTYPE html>` 或者没有html4的文档声明时浏览器的排版会模拟Navigator4 与ie5的非标准行为，为了支持在新的web标准被广泛使用前就已经建好的网站这是必须的（我们常说的网站向后兼容）。

以前不同的浏览器实现了不同的quirks，特别是在ie6,7,8,9中，Quirks模式被冻结在IE5.5中。在不同的浏览器中，Quirks模式和Almost Standards模式有一定的偏差。在IE10中的Quirks不再是模仿IE5.5，而是寻找与其他浏览器的Quirks模式的相互操作。

区别于IE10中模仿IE5.5的怪癖模式，IE10和其他浏览器的模式被称为 `互操作Quirks模式`。

### 标准模式

浏览器排版按照HTML与CSS的规范描述的行为来执行

### 几乎标准模式

只有部怪异的行为被实现。

Firefox，Safari，Chrome，Opera（从7.5开始），IE8，IE9和IE10也有一种称为“几乎标准模式”的模式，它实现了传统上不依赖于CSS2规范的表单元的垂直大小。Mac IE 5，Windows IE 6和7，7.5之前的Opera以及Konqueror都不需要几乎标准模式，因为它们并没有根据各自标准模式下的CSS2规范来实现表格单元的垂直大小。实际上，它们的标准模式比较新的浏览器的标准模式更接近于标准模式

注：
请确定你把 `DOCTYPE` 正确地放在 `HTML` 文件的顶端。任何放在 `DOCTYPE` 前面的东西，比如注释或 XML 声明，会令 Internet Explorer 9 或更早期的浏览器触发怪异模式

### 标准模式和怪异模式的区别

标准模式

	盒子模型 width/height= width + 2*margin-left + 2*padding-left + 2*border-left

怪异模式
	
	盒子模型 width/height= width + 2*margin-left

知识点扩展 XHTML

如果你的网页使用XHTML并在 Content-Type HTTP 标头使用application/xhtml+xml MIME 类型，你不需要使用 DOCTYPE 启动标准模式，因为这种文件会永远使用标准模式。不过请注意，页面使用 application/xhtml+xml 会令 Internet Explorer 8 出于未知格式之故出现下载对话框，因为支持 XHTML 的第一个 Internet Explorer 版本是 Internet Explorer 9。

如果你的类 XHTML 网页使用 text/html MIME 类型，浏览器会将其视为 HTML，因此你就需要 DOCTYPE 启用标准模式


用 `JavaScript` 获取浏览器的模式：

`document.compatmode` 有两个可能值 BackCompat（怪异模式），CSS1Compat（标准模式）
