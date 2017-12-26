## `doctype` 是什么？

### 定义和使用

DOCTYPE的作用是告诉浏览器你所编写的页面是什么版本。它应该在你的HTML文档中第一位出现，如果你使用的是HTML5,它应该像这样：<!DOCTYPE html>，相对于[其他9种类型](https://en.wikipedia.org/wiki/Document_type_declaration)它是最简洁的，DOCTYPE不是一个元素标签。

### 为什么HTML5没有DTD？

HTML5的声明很短，没有引用`DTD`（文档类型定义)，HTML5不是基于SGML的。而在 `HTML 4.0.1` 中 `<!DOCTYPE>` 声明引用一个 `DTD`，因为 `HTML 4.0.1` 是基于 `SGML` 的，浏览器根据 `DTD` 对标记语言的规则才能正确的渲染文档内容。

### HTML4 几种doctype

	HTML 4.01 Strict

	<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">

	HTML 4.01 Transitional

	<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">

	HTML 4.01 Frameset

	<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Frameset//EN" "http://www.w3.org/TR/html4/frameset.dtd">

`HTML` 是文档的根元素，正如我们经常见到的 `<html></html>`，然后其他的内容都进一步的描述了DTD。由于HTML5不使用DTD，所以没有其他东西可以声明。

HTML5的设计者认为DTDs的表现力有限，以及HTML5的的验证（基本的HTML5模型[http://validator.nu](http://validator.nu)，[http://validator.w3.org](http://validator.w3.org)）用模式和临时检查，而非基于DTD的验证。

此外，HTML5的设计本身已经使它对于写一个DTD不起作用。例如，没有SGML能获取任何以`data-`开头和编译的符合一般规则的属性的HTML5规则。在SGML中，所有属性需要单独列出，所以，DTD将被无限次使用。

DTD的限制太多，太复杂，需要很多的语法错误捕获。

想要更深层次的了解HTML5为什么没有使用DTD，[StackOverflow](http://stackoverflow.com/questions/4053917/where-is-the-html5-document-type-definition)的回答很详尽。

后来问着问着扩展到SVG，XML

## SVG

### 定义

SVG 意为可缩放矢量图形（Scalable Vector Graphics）

SVG使用XML格式定义图像

虽然 `SVG` 解决了大量的元素冲突问题，但doctype从未处理多个被指定的文件解析到一个文档中的情况，这时你应该使用 `xmlns` 属性（XML namespace）。就 `SVG` 而论，标准的 `SVG` 如下：
	
	<svg xmlns="http://www.w3.org/2000/svg">
	    <!-- SVG-specific tags here -->
	</svg>

### 用途

由于svg是基于矢量的所以它能很好的处理图形大小的改变，svg是基于XML的所以svg比较适合做动态交互，相对于canvas绘图它有这样的优势。

canvas提供的功能更原始，适合像素处理，动态渲染和大数据绘制

svg提供的功能更完善，适合静态图片展示，高保真文档和打印的应用场景

[canvas vs svg](http://ycoder.com/2013/12/)

[svg教程](http://www.w3school.com.cn/svg/)

## XML

### 定义

XML 指可扩展标记语言（eXtensible Markup Language）。

### 用途

XML 被设计用来传输和存储数据。

### XML 和 HTML 的区别

XML 和 HTML 为不同的目的而设计：
	
	XML 被设计用来传输和存储数据，其焦点是数据的内容。
	HTML 被设计用来显示数据，其焦点是数据的外观。
	HTML 旨在显示信息，而 XML 旨在传输信息。

### 知识点扩展

SGML：标准通用标记语言（以下简称“通用标言”），是一种定义电子文档结构和描述其内容的国际标准语言；通用标言为语法置标提供了异常强大的工具，同时具有极好的扩展性，因此在数据分类和索引中非常有用；是所有电子文档标记语言的起源，早在万维网发明之前“通用标言”就已存在。
