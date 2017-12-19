# 前端开发中关于icon 使用的总结

## 解决方案：
	1. Css sprite 雪碧图，也称为css精灵图
	2. Icon font 图标字体
	3. Data URI scheme
	
	
## 实现原理：
#### 雪碧图	它的基本原理是将网站中的所有的图标图片整合到一张图片中，该图片使用css 中的width,height,background属性和background-position属性来渲染网站中请求页面中的图标。
#### Icon font	 它的基本原理自定义一种字体，将不同的字配置作为icon图案，然后通过css嵌入其中使用。 
#### DataURI scheme	Data URI scheme是在RFC2397中定义的，将一些小的数据，直接嵌入到网页中，从而不用再从外部文件载入


## 优势&劣势
### 雪碧图
> 优势

 		1. 加快网页加载速度
		2. 能减少图片的字节数
		3. 解决了前端工程师的命名问题
		4. 便于后期维护，更换方便
		5. 可以避免鼠标滑过的一些bug(IE6)
>  劣势
		
	1. 占据内存（大量无用的空白）
    2. 雪碧图拼图制作麻烦，
    3. 影响浏览器的缩放功能
	4. 雪碧图调用的图片不能被打印，除非在@media中特殊说明
	5. 为了减少 HTTP 请求数(这是使用 CSS Sprite 一直强调的好处)，然而把所有的图片都当背景图片来处理，尤其是那些传达重要信息的图片，将会导致一个网站缺乏可访问性的问题，也会降低 HTML 中 title 和 alt 潜在的好处。其实，CSS sprite 本身并不会出错，也不会引发可访问性问题(事实上，正确得使用会提高可访问性)。
> 不适合使用雪碧图的场景

    1. 网络环境比较差
	2. 移动页面夜间模式下


### ICON font
> 优势 

    1. 很好的解决了响应式设计中图形无损自适应的问题，可以通过font-size和color属性来控制icon的大小和颜色
    2. 体积小，可以无限拉伸
> 劣势

    1. 样式单一、颜色单一
	2. 有少量的移动设备可能会和icon fonts的字符编码冲突，导致icon显示不正常
	3. 跨域问题
	4. 文字图片的大小设置
	5. 10p以下的字体chrome浏览器显示12px

### Data URI scheme
>优势

    使用一个data uri scheme
>劣势

    浏览器使用的过程中不会缓存该图片

## 参考资料：
> Sprite

    http://www.cnblogs.com/hustskyking/archive/2015/08/17/iconfont-opt.html
    http://www.cnblogs.com/demix/archive/2009/11/28/1612715.html
    https://segmentfault.com/q/1010000000407231
    http://ntx.me/2015/05/21/IconFont/
    https://think2011.net/2017/03/31/css-sprite/

> Iconfont

    http://www.cnblogs.com/hustskyking/archive/2015/08/17/iconfont-opt.html
    http://www.cnblogs.com/demix/archive/2009/11/28/1612715.html
    https://segmentfault.com/q/1010000000407231
    http://ntx.me/2015/05/21/IconFont/

>Data URI scheme

	 Data URI中，data表示取得数据的协定名称，image/png 是数据类型名称，base64 是数据的编码方法，逗号后面就是这个image/png文件base64编码后的数据。
     Data URI scheme支持的类型有：
        data: ,文本数据
        data:text/plain,文本数据
        data:text/html,HTML代码
        data:text/html;base64,base64编码的HTML代码
        data:text/css,CSS代码
        data:text/css;base64,base64编码的CSS代码
        data:text/javascript,Javascript代码
        data:text/javascript;base64,base64编码的Javascript代码
        data:image/gif;base64,base64编码的gif图片数据
        data:image/png;base64,base64编码的png图片数据
        data:image/jpeg;base64,base64编码的jpeg图片数据
        data:image/x-icon;base64,base64编码的icon图片数据

      
      http://blog.csdn.net/c_mihoo/article/details/12774719
      https://isux.tencent.com/understand-data-uri-performance.html
