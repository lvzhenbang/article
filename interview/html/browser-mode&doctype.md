## yongdoctype激活浏览器模式

为了解决使用上世纪90年代后期以传统方式编写的web内容和根据web标准编写的web内容兼容性问题，现代浏览器采用了各种引擎模式。下面就来详细介绍这些模式以及如何触发它们。

### text/html内容的共同模式

text/html内容模式的选择取决于doctype嗅探（稍后在本文档中讨论）。在IE8及更高版本中，模式也取决于其他因素。但是，即使在IE8及更高版本中，默认情况下，模式也取决于非Intranet站点的文档类型，这些站点不在Microsoft提供的黑名单中。另外，如果安装了Google Chrome浏览器框架，即使使用IE6和IE7也会涉及其他因素。

怪癖模式， 标准模式，几乎标准模式

详细介绍请参考[浏览器渲染html文档的有几种模式](https://github.com/lvzhenbang/article/tree/master/interview/browser-mode.md)

### application/xhtml+xml内容模式（XML模式）

在Firefox，Safari浏览器，Chrome浏览器，Opera和IE9的application/xhtml+xml HTTP Content-Type（不是一个meta元素，也不是<！DOCTYPE>）会触发XML模式。在XML模式下，这些浏览器在特定的浏览器中实现了对XML文档的规范正确的处理。

IE 6,7和8不支持application/xhtml+xml。Mac IE 5也没有。

### IE特定的微软附加模式

以下是HTML5以外的其他特定于IE的模式，其他浏览器不具有这些模式。它们的激活涉及到配置，或者X-UA-Compatible作为HTTP头或meta元素（下面讨论）。

Internet Explorer 5的怪癖

除了可互操作的Quirks模式之外，IE10还有一个名为“Internet Explorer 5 Quirks”的模式，它模仿IE 5.5，在IE6，IE7，IE8和IE9中被称为Quirks模式。

Internet Explorer 7标准
IE8，IE9和IE10有一个模仿IE7标准模式的模式。

Internet Explorer 8标准
IE9和IE10有一个模仿IE8标准模式的模式。

Internet Explorer 8几乎标准
IE9和IE10有一个模式，模仿IE8中的几乎标准模式。在开发人员工具用户界面中，此模式与“Internet Explorer 8标准”没有区别。

Internet Explorer 9标准
IE10具有模仿IE9中标准模式的模式。

Internet Explorer 9几乎标准
IE10有一个模仿IE9中的几乎标准模式的模式。在开发人员工具用户界面中，该模式与“Internet Explorer 9标准”没有区别。

Internet Explorer 9 XML
IE10有一个模仿IE9中XML模式的模式。在开发人员工具用户界面中，该模式与“Internet Explorer 9标准”没有区别。

值得注意的是，以前版本的IE的模仿并不完美。例如：包括IE7标准在后续的IE版本处理@font-face模拟不同的EOT字体和支持CSS 2D转换的IE9模式，IE10不需要-ms-, 但IE9需要前缀。如果你按照本文所给出的建议，你不会针对这些模式，所以模仿的不完美对你在生产中是无关紧要的。然而测试的结果是运行在一堆虚拟机上，不如实际的旧版IE中测试你的站点比使用开发者工具更新的IE版本更好地使新版本模拟旧版本进行测试更好。

Windows Phone 8的IE10也具有所有这些模式，就像桌面上的IE10一样。

### Google特定于IE的附加模式

在安装Google Chrome浏览器框架时，以下是IE6，IE7，IE8和IE9中的其他可用模式（但不包括Windows 8上的IE10或Windows 7中的IE10）。

Chrome怪癖
此模式与Google Chrome中的Quirks模式相同。

Chrome标准
此模式与Google Chrome中的标准模式相同。

铬几乎标准
这种模式与Google Chrome中的几乎标准模式相同。

### 非Web模式

一些引擎具有与Web内容无关的模式。这些模式仅在这里提到的完整性。Opera有一个WML 2.0模式。Mac OS X 10.5上的WebKit对传统仪表板小部件有一个特殊的模式（也许这种模式保持在较新的版本 - 我没有调查过）。WebKit也在Mac OS X上嵌入了WebKit的应用程序。

### 布局

除了在IE浏览器，在模式text/html主要影响CSS布局和风格体系。例如，不把样式继承到表是一个怪癖。在IE和Opera的旧版本中，盒子模型在Quirks模式下变为IE 5.5盒子模型。本文没有列举所有的布局怪癖。有关列表，请参阅Mozilla的文档和怪癖模式规范。
在几乎标准模式下（在具有当前标准模式的浏览器中），与标准模式相比，仅包含图像的表格单元格的高度不同。
在XML模式下，选择器具有不同的区分大小写行为。此外，HTML body元素的特殊规则不适用于旧版浏览器，这些浏览器没有实现对CSS规范的更新调整。

### 解析

也有一些怪癖影响HTML和CSS解析，并会导致合格的页面被错误分类。这些怪癖是打开和关闭与古怪的布局。然而，重要的是要认识到，怪癖模式与标准模式主要是关于CSS布局和CSS解析 - 而不是HTML解析。在具有HTML5兼容的HTML解析器的浏览器中，恰好有一个HTML解析怪癖。
有些人误导性地将标准模式称为“严格分析模式”，这被误解为暗示浏览器强制执行HTML语法规则，并且可能使用浏览器来评估标记的正确性。这是不是这种情况。即使在标准模式布局有效的情况下，浏览器也会对汤进行标记。（在Netscape 6发布之前的2000年夏天，Gecko实际上拥有强制执行HTML语法规则的解析器模式，其中一种模式被称为“严格DTD”，这些模式与现有的Web内容不兼容，并被放弃。
另一个常见的误解是与XHTML解析有关。通常认为使用XHTML文档可以获得不同的解析。它不是。用作HTML的XHTML文档text/html使用与 HTML 相同的解析器进行解析。就浏览器而言，XHTML text/html只是“用油煎面包块标记汤”（在这里和那里额外的斜线）。只有使用XML内容类型（例如application/xhtml+xml或application/xml）的文档才会触发XML模式进行分析，在这种情况下，分析器与HTML分析器完全不同。

### 脚本

虽然怪异模式主要是关于CSS，但也有一些脚本怪癖。在Firefox 14之前，HTML id属性并没有在标准和几乎标准模式下从全局脚本范围中建立对象引用。在Firefox中，document.all在Quirks模式下部分可用，但在其他模式下不可用。当进入一个模拟旧版IE的模式时，IE对脚本的影响更为显着。
在XML模式下，一些DOM API的行为不同，因为XML的DOM API行为被定义为与HTML行为不兼容。事后看来，这是相当不幸的。

### Doctype嗅探（又名.Doctype切换）

浏览器使用doctype嗅探来决定text/html文档的引擎模式。这意味着模式是基于HTML文档开始处的文档类型声明（或者缺少）而被选取的。（这不适用于使用XML内容类型的文档。）
文档类型声明（doctype）是SGML的一种语法工件 - 一种传统的标记框架，HTML5之前的HTML据称是按照定义的。在HTML 4.01规范中，文档类型声明被称为传递HTML 版本信息。尽管名称“文档类型声明”，尽管什么HTML 4.01规范说，关于“版本信息”，文档类型声明不是SGML或XML文档归类为特定类型的文件的适当的手段，即使现在看来，这本来是（因此而得名）。（更多信息请见附录。）
HTML 4.01规范和ISO 8879（SGML）都没有提到将文档类型声明用作引擎模式开关的任何内容。Doctype嗅探是基于这样一个观察，即在设计doctype嗅探时，绝大多数古怪的文档或者没有文档类型声明，或者他们引用了旧的DTD。HTML5承认这一现实，并将doctype定义text/html为模式切换。
甲典型预HTML5文档类型声明包含（由空格分隔）字符串“ <!DOCTYPE”，根元素的通用标识符（“ html”），字符串“ PUBLIC”，一个DTD的在引号中的公共标识符，可能是系统标识符（一个URL）相同的DTD和字符“ >”。HTML5将doctype简化为“ <!DOCTYPE html>”。文档类型声明放在根元素的开始标签之前的文档中。

text/html
简而言之： 以下是为新text/html文档选择文档类型的简单指南：
标准模式，尖端的验证
<!DOCTYPE html>

这是你应该使用的。有了这个文档类型，可以验证新功能，比如<video>，<canvas>和ARIA。请确保在最新版本的顶级浏览器中测试您的页面。

标准模式，传统验证目标
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">

这个文档类型也会触发标准模式，但是如果您的组织具有需要锁定传统验证的愚蠢策略，那么您可以坚持使用不太精确的旧版验证，这些验证不知道新功能。但是你真的应该使用<!DOCTYPE html>和修改你的组织的政策。

您想要使用标准模式，但是您在表格布局中使用切片图像，并且不想修复它们
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">

这给你几乎标准模式。请注意，如果您稍后移至HTML5（因此，完整的标准模式），基于表格中切片图像的布局可能会中断，因此现在最好让您的设计标准模式兼容。

你故意要怪癖模式
没有文档类型。

请不要这样做。有意设计怪癖模式将在未来困扰着你，你的同事或你的接班人。

如果您对旧IE版本之间的差异感到沮丧，并且仍然需要根据客户端的要求来支持它们，那么使用[条件注释](https://www.quirksmode.org/css/condcom.html)对遗留版本应用特定的黑客比在Quirks模式中寻求共同性更好。

我不推荐任何XHTML文档类型，因为服务XHTML text/html被认为是有害的。如果您选择使用XHTML文档，请注意XML声明使得IE 6（而不是IE 7！）触发了Quirks模式。
application/xhtml+xml
对于简单的指引application/xhtml+xml是不使用DOCTYPE 在所有。这样页面就不能“严格符合”XHTML 1.0，但这并不重要。

IE8，IE9和IE10复杂功能
它在A List Apart上宣布meta，除了doctype是模式选择的一个因素之外，IE8还将使用基于元素的模式开关。（请参阅Ian Hickson，David Baron，David Baron，Robert O'Callahan和Maciej Stachowiak的评论。）
IE8有四种模式：IE 5.5 quirks模式，IE 7标准模式，IE 8几乎标准模式和IE 8标准模式。IE9有七种模式：IE5.5怪癖模式，IE7标准模式，IE8几乎标准模式，IE8标准模式，IE9 几乎标准模式，IE9 标准模式和IE9 XML模式。IE10有十一种模式：IE5.5 quirks模式，IE7标准模式，IE8几乎标准模式，IE8标准模式，IE9几乎标准模式，IE9标准模式，IE9XML模式，IE10 怪癖模式，IE10 几乎标准模式，IE 10标准模式和IE 10 XML模式。模式的选择取决于来自各种来源的数据：doctype，ameta元素，HTTP头，从Microsoft定期下载的数据，Intranet区域，用户所做的设置，由Intranet管理员所做的设置，框架父母的模式（如果有的话）以及用户可切换的UI按钮。（对于嵌入引擎的其他应用程序，模式也取决于嵌入应用程序。）
幸运的是，IE8和IE9使用doctype嗅探大致像其他浏览器和IE10使用doctype嗅探完全像其他浏览器，如果所有以下几点是正确的：
没有X-UA-Compatible由作者设置的HTTP标头。
没有X-UA-Compatible meta标签由作者设置。
微软并没有把网站的域名放在黑名单上。
内部网管理员没有将该站点放在黑名单上。
用户没有按下兼容性视图按钮（或以其他方式将域添加到用户特定的黑名单）。（Metro IE10没有这个用户界面，但桌面IE10中的用户界面也会影响Metro模式下的行为。）
该网站不在Intranet区域。
用户没有选择像在IE7中显示的所有网站。
该页面不是由“兼容模式”页面构成的。
除了这两种X-UA-Compatible情况以外，IE8和IE9都像IE7一样执行doctype嗅探。IE7仿真称为兼容性视图。
在这种X-UA-Compatible情况下，IE8和IE9的行为与其他浏览器截然不同。有关IE8的行为，请参阅此页面上的附录或以PDF和PNG格式提供的流程图。（与其他浏览器的图表作为PDF对比）。还有一个统一的IE 5.5到9（可能与Chrome Frame）模式的图表作为PDF。（我打算以后只能在IE9中查看这个图表。）
不幸的是，如果没有X-UA-CompatibleHTTP标头或meta标签，即使您使用了适当的文档类型，IE8和IE9也会让您的页面从最标准的模式意外掉到模拟IE7标准模式的IE7模式。更糟糕的是，内部网管理员可能会这样做。此外，微软可能已经将您使用的整个域名列入黑名单（例如mit.edu！）。
为了抵消这些影响，文档类型是不够的，你需要一个X-UA-CompatibleHTTP头或meta标签。
简而言之： 下面是为一个新的文档选择一个X-UA-CompatibleHTTP头或meta标签的简单指南，这个 text/html文档已经有了一个在其他浏览器中触发标准模式或几乎标准模式的文档类型：
你的域不在微软的黑名单上，你更关心的不是浏览器特有的错误，而是确保用户无法将渲染回归到IE7行为
您不需要包含X-UA-CompatibleHTTP标头或meta标签。

您的域位于微软的黑名单，您的域名（如iki.fi！）有其他作者，其网站可能会导致用户启用整个域的兼容性视图，您担心Google或Digg构架您的网站，或者您想确保用户无法启用兼容性视图
meta在页面上<meta http-equiv="X-UA-Compatible" content="IE=Edge">（在任何script元素之前）包含以下元素（在HTML5中无效）或在页面上设置以下HTTP标题：X-UA-Compatible: IE=Edge

您的网站在IE7中工作，但在IE8或IE9中断
首先，meta在页面上<meta http-equiv="X-UA-Compatible" content="IE=EmulateIE7">（在任何script元素之前）包含以下元素（在HTML5中无效）或在页面上设置以下HTTP标题：X-UA-Compatible: IE=EmulateIE7

然后修复您的网站不要依靠非标准的IE7行为并迁移到IE=Edge。

您的网站在IE8中工作，但在IE9中断
首先，meta在页面上<meta http-equiv="X-UA-Compatible" content="IE=EmulateIE8">（在任何script元素之前）包含以下元素（在HTML5中无效）或在页面上设置以下HTTP标题：X-UA-Compatible: IE=EmulateIE8

然后修复您的网站不要依靠非标准的IE8行为并迁移到IE=Edge。

您的网站在IE9中工作，但在IE10中断
首先，meta在页面上<meta http-equiv="X-UA-Compatible" content="IE=EmulateIE9">（在任何script元素之前）包含以下元素（在HTML5中无效）或在页面上设置以下HTTP标题：X-UA-Compatible: IE=EmulateIE9

然后修复您的网站不要依靠非标准的IE8行为并迁移到IE=Edge。

谷歌浏览器框架并发症
Google Chrome Frame是IE 6,7,8和9的浏览器扩展和浏览器插件的组合，它使用IE使用的网络堆栈将Google Chrome引擎添加到IE的用户界面外壳中。安装后，IE默认正常运行。但是，网页可以选择使用X-UA-CompatibleHTTP头或元标记来调用Chrome的引擎，而不是IE的引擎。
如果Chrome Frame已安装，则指定chrome=1在X-UA-Compatible任何支持的IE版本中调用Chrome Frame。指定chrome=IE6仅在IE6中激活Chrome Frame，指定chrome=IE7仅在IE7和IE6中chrome=IE8激活Chrome Frame ，并且仅在IE8及更低版本中激活Chrome Frame。
用于激活Chrome Frame的指令可以通过用逗号或分号分隔它们来与控制IE引擎的指令（如果Chrome Frame未安装）结合使用：<meta http-equiv="X-UA-Compatible" content="IE=Edge,chrome=IE8">。
一旦Chrome框架被激活了一个页面，铬的四个模式（标准，几乎标准，怪癖和XML）中的一个被选择为正常的Chrome。
使用Chrome Frame有几个重要的原因：
Chrome框架缺少IE的可访问性支持。当Chrome浏览器框架被激活时，在IE中的内容区域成为一个可访问性的黑洞。也就是说，屏幕阅读器和Windows语音识别不适用于Chrome Frame。
让您的网站告诉用户，他们应该安装Chrome框架，以延长网站的安全反模式，告诉用户他们应该让别人在他们的计算机上安装特权的本机代码插件，以便使用网站。