## DOM的发展，DOM0,DOM1,DOM2,DOM3

Documentc Object Model文档对象模型是针对XML但经过扩展用于HTML的应用程序接口（API Application programming Interface).DOM把整个界面都映射成多层次节点结构，每个组成部分都是某种类型的节点，通过DOM可以操作任何节点。

### DOM由来

因为Internet Explorer4和Netscape Navigation4分别支持不同的DHTML（动态HTML）,为了统一标准，负责制定web通信标准的W3C(World Wide Web Consortium,万维网联盟)开始制定DOM.

### DOM0

Netscape Navigator 4和IE4分别发布于1997年的6月和10月发布的DHTML，他们是未形成标准的试验性质的初级阶段的DOM，称为dom0,并不是标准。

### DOM1

DOM1是W3C在1998年制定的标准，DOM1级主要定义了HTML和XML文档的底层结构。在DOM1中，DOM由两个模块组成：DOM Core（DOM核心）和DOM HTML。其中，DOM Core规定了基于XML的文档结构标准，通过这个标准简化了对文档中任意部分的访问和操作。DOM HTML则在DOM核心的基础上加以扩展，添加了针对HTML的对象和方法，如：JavaScript中的Document对象.

### DOM2

DOM2级在原来DOM的基础上又扩充了鼠标、用户界面事件、范围、遍历等细分模块，而且通过对象接口增加了对CSS的支持。DOM1级中的DOM核心模块也经过扩展开始支持XML命名空间。在DOM2中引入了下列模块，在模块包含了众多新类型和新接口：

DOM视图（DOM Views）：定义了跟踪不同文档视图的接口
DOM事件（DOM Events）：定义了事件和事件处理的接口
DOM样式（DOM Style）：定义了基于CSS为元素应用样式的接口
DOM遍历和范围（DOM Traversal and Range）：定义了遍历和操作文档树的接口

### DOM3

DOM3进一步扩展了DOM，在DOM3中引入了以下模块：

DOM加载和保存模块（DOM Load and Save）：引入了以统一方式加载和保存文档的方法
DOM验证模块（DOM Validation）：定义了验证文档的方法
DOM核心的扩展（DOM Style）：支持XML 1.0规范，涉及XML Infoset、XPath和XML Base
