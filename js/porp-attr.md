## 傻傻的分也分不清楚的property和attribute

最近，一个小伙伴问了我一个问题property和attribute的区别？当时我想了又想，很不好意思的说了我不知道，所以，抽了个事件好好的利用了一下‘度娘’和‘Google’总结了一下。度娘搜索到的有用信息[知乎中的讨论](https://www.zhihu.com/question/30111950?sort=created)[csdn搜索的结果](http://blog.csdn.net/bugknightyyp/article/details/17166155)，Google发现的有用信息，[lucy女神的解释](http://lucybain.com/blog/2014/attribute-vs-property/) [JOJI.ME中的解释](http://joji.me/en-us/blog/html-attribute-vs-dom-property)通过查看这些我们发现要分辨property和attribute并非那么难。

### property和attribute的真实含义

property它是相对于对象本身来说的，本身就是对象的组成部分，也可以说是对象本身

property用人来类比就好像是人有眼，手，耳等器官一样，人生而有之（先天的）

attribute它是相对于对象来说的，本身只是对对象的一种说明，就好像是我们在朋友聚会时对彼此不认识的伙伴的介绍一样。

attribute用人来类比就好像是人有名字，学历，女朋友等（后天的）

### javascript使用中我们应该注意的事项

property 是有类型的，例如Bollean，number，string等

attribute 只能是string，而没有其他类型

### 使用中应注意的事项

使用过jQuery的同学都知道，在jQuery中有.prop()和.attr()这样两个钩子函数，有相当一部分同学只是知道有这样两个方法，但该如何的使用一些同学还是傻傻的分也分不清楚？对于那些说只要会用能解决问题就行了的观点，我也只能一笑而过。
	
	// dom
	<div class="switch">
		<input type="radio" checked value="1" name="sex">男
		<input type="radio" value="0" name="sex">女
	</div>

在示例中input元素的type，checked，name都是input元素对象的property，我们可以像给一半元素对象赋值一样修改input的对象的property,通过下面的代码：
	
	document.querySelectorAll('input')[1].type = 'text'

会得到这样的结果

![prop修改](https://github.com/lvzhenbang/article/blob/master/img/js/prop.gif)

	document.querySelectorAll('input')[0].setAttribute('type', 'text')

attribute修改会得到这样的结果

![attr修改](https://github.com/lvzhenbang/article/blob/master/img/js/attr.gif)

我们发现结果都改变了input对象的类型，难道是巧合，确实是巧合，因为prop和attr都可以设置字符串的属性，因为是字符串所以都起效

如果我们修改checked的值将只有prop起作用，因为checked的属性值是Boolean类型的如下图

![checked修改](https://github.com/lvzhenbang/article/blob/master/img/js/attr.gif)	
	document.querySelectorAll('input')[0].checked = false
	
	document.querySelectorAll('input')[1].setAttribute('checked', false)

我们发现 setAttribute('checked', false)并不是不起作用，而是被选中了，说明了什么，说明了input元素对象将checked值设为了‘false’字符串，而在js中非空的String类型会根据执行的语境别转换类型为Boolean类型true


### jQuery中的prop()和attr()源码的分析
	
jquery version 3.2.1
	
.prop()原码分析

	jQuery.fn.extend( {
		prop: function( name, value ) {
			return access( this, jQuery.prop, name, value, arguments.length > 1 );
		},

		removeProp: function( name ) {
			return this.each( function() {
				// 删除名字为name的property
				delete this[ jQuery.propFix[ name ] || name ];
			} );
		}
	} );

	jQuery.extend( {
		prop: function( elem, name, value ) {
			...
			if ( value !== undefined ) {
				...
				// 为名字为name的property属性赋值
				return ( elem[ name ] = value );
			}
			...
			// 返回名字为name的property属性
			return elem[ name ];
		},
		...
	} );

.attr()源码分析

	jQuery.fn.extend( {
		attr: function( name, value ) {
			return access( this, jQuery.attr, name, value, arguments.length > 1 );
		},

		removeAttr: function( name ) {
			return this.each( function() {
				jQuery.removeAttr( this, name );
			} );
		}
	} );

	jQuery.extend( {
		attr: function( elem, name, value ) {
			...
			if ( value !== undefined ) {
				if ( value === null ) {
					jQuery.removeAttr( elem, name );
					return;
				}

				if ( hooks && "set" in hooks &&
					( ret = hooks.set( elem, value, name ) ) !== undefined ) {
					return ret;
				}

				elem.setAttribute( name, value + "" );
				return value;
			}

			if ( hooks && "get" in hooks && ( ret = hooks.get( elem, name ) ) !== null ) {
				return ret;
			}			
			// 取得并返回名字为name的attribute
			ret = jQuery.find.attr( elem, name );
			return ret == null ? undefined : ret;
		},
		...
		removeAttr: function( elem, value ) {
			...
			if ( attrNames && elem.nodeType === 1 ) {
				while ( ( name = attrNames[ i++ ] ) ) {
					// 元素elem删除名字为name的attribute
					elem.removeAttribute( name );
				}
			}
		}
	} );

从上面的实例代码我们不难看出jQuery源码对attribute和property的创建和删除进行了严格的区分，attribute的创建和删除用setAttribute(), getAttribute();而property的创建和删除使用的方式跟一般对象一致，obj[name] = [value] 创建并赋值，delete obj[name] 则删除

通过上面的示例我们发现setAttribute()也可以对一些property进行修改，这一点我们稍后再说，但无疑jQuery的做法把开发变得简单了起来，

### 为attr和prop赋值&取值

通过上一小节的代码我们知道，我们可以通过两种方式向dom对象的属性来赋值。

1.像给一半的js对象那样给dom对象的属性赋值，取值跟一般对象一样；

2.通过setAttribute()为dom对象赋值，取值通过getAttribute()

第一种方法可以为dom对象赋值和取值，但能反应在浏览器dom结构中的只有元素自带的属性

第二种方法不仅可以为dom对象赋值和取值，还可以在浏览器的dom结构中来显示自定义的dom对象属性

下面是一张示例图：

![属性的赋值&取值](https://github.com/lvzhenbang/article/blob/master/img/js/get_set_attr.png)

### jQuery中的prop&attr

jQuery作为一代经典的前端库，我们曾经在判断input单选和复选框是否被选中
用if(doucment.querySelector('input').checked)，现在用if($('input').prop(
'checked'))

jQuery对prop()和attr()做了严格的区分，prop就是对象的属性，attr就是描述属性

![.prop()和.attr()一些均可以获取的属性](https://github.com/lvzhenbang/article/blob/master/img/js/prop+attr.png)

纠错：type属性两方法以均可获取和设置相应的值

图片来源于：[https://stackoverflow.com/questions/5874652/prop-vs-attr/5884994#5884994](https://stackoverflow.com/questions/5874652/prop-vs-attr/5884994#5884994)

### 友情提示

* 在定义自定义属性是建议为 data-[属性名] 这种形式，相信写过jQuery插件或修改研究过 jQuery插件的同学都不陌生。

* DOM property 和 HTML attribute

* 通过document.querySelector[0].attributes 可以获取dom对象的所有attribute属性

* 有些自定义的attribute，也可通过(.)这种方式来获取

### 推荐

![HTML Attributes,Mozilla Dev Center](https://developer.mozilla.org/en/HTML/Attributes)

![DOM Element Properties,Mozilla Dev Center](https://developer.mozilla.org/en/HTML/Properties)

注：要深入了解的同学可以看一看
