## `doctype` 是什么？

### 定义和使用

DOCTYPE的作用是告诉浏览器你所编写的页面是什么版本。它应该在你的HTML文档中第一位出现，如果你使用的是HTML5,它应该像这样：<!DOCTYPE html>，相对于[其他9种类型](https://en.wikipedia.org/wiki/Document_type_declaration)它是最简洁的，DOCTYPE不是一个元素标签。

### 第一问：为什么HTML5没有DTD？

HTML5的声明很短，没有引用`DTD`（文档类型定义)，HTML5不是基于SGML的。而在 `HTML 4.0.1` 中 `<!DOCTYPE>` 声明引用一个 `DTD`，因为 `HTML 4.0.1` 是基于 `SGML` 的，浏览器根据 `DTD` 对标记语言的规则才能正确的渲染文档内容。

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

## SVG

SVG 意为可缩放矢量图形（Scalable Vector Graphics）

SVG使用XML格式定义图像

虽然 `SVG` 解决了大量的元素冲突问题，但doctype从未处理多个被指定的文件解析到一个文档中的情况，这时你应该使用 `xmlns` 属性（XML namespace）。就 `SVG` 而论，标准的 `SVG` 如下：
	
	<svg xmlns="http://www.w3.org/2000/svg">
	    <!-- SVG-specific tags here -->
	</svg>

[svg教程](http://www.w3school.com.cn/svg/)