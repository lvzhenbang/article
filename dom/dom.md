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

### dom事件

IE的事件流是冒泡, 从里层往最外层冒, netscape是从外层元素往内层元素的捕获;

dom2 事件规定了事件流的三个阶段：1.事件捕获，2.处于目标，3.事件冒泡 

事件兼容性处理方案

	var eventUtil = {
        addEvent : function(el, type, handler) {
            if(el.addEventListener) {
                el.addEventListener(type, handler, false);
            }else if( el.attachEvent ) {
                el.attachEvent("on"+type, handler);
            }else{
                el["on"+type] = handler;
            }
        },
        offEvent : function(el, type, handler) {
            if( el.removeEventListener ) {
                el.removeEventListener(type, handler, false)
            }else if( el.detachEvent ) {
                el.detachEvent(type, handler);
            }else{
                el["on"+type] = null;
            }
        }
     };

> 时间对象

	事件对象event下的属性和方法：因为各个浏览器的事件对象不一样, 把主要的时间对象的属性和方法列出来;

	bubble：表明事件是否冒泡
	cancelable： 表明是否可以取消冒泡
	currentTarget：当前时间程序正在处理的元素, 和this一样的;
	defaultPrevented： false，如果调用了preventDefualt这个就为真了;
	detail： 与事件有关的信息(滚动事件等等)
	eventPhase：如果值为1表示处于捕获阶段，值为2表示处于目标阶段，值为三表示在冒泡阶段
	target || srcElement：事件的目标
	trusted：为ture是浏览器生成的，为false是开发人员创建的（DOM3）
	type：事件的类型
	view：与元素关联的window，我们可能跨iframe；
	preventDefault(): 取消默认事件；
	stopPropagation(): 取消冒泡或者捕获；
	stopImmediatePropagation(): (DOM3)阻止任何事件的运行；
	// topImmediatePropagation阻止绑定在事件触发元素的其他同类事件的callback的运行
	 
	IE下的事件对象是在window下的，而标准应该作为一个参数, 传为函数第一个参数;
	IE的事件对象定义的属性跟标准的不同，如：

	cancelBubble 默认为false, 如果为true就是取消事件冒泡;
	returnValue 默认是true，如果为false就取消默认事件;
	srcElement 这个指的是target, Firefox下的也是srcElement;

