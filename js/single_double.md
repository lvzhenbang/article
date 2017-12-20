## 给一个元素同时绑定click和dbclick存在的问题

这是一个我面试的面试题

### 代码实例

	<!DOCTYPE html>
	<html lang="en">
	<head>
		<meta charset="UTF-8">
		<title>demo</title>
	</head>
	<body>

		<button id="my_btn">click it</button>

	<script type="text/javascript" src="https://code.jquery.com/jquery-1.11.1.min.js"></script>
	<script type="text/javascript">
		$("#my_btn").click(function() { 
	        alert('single click');
	    });

		$("#my_btn").dblclick(function() {
	        alert('double click');
	    });
	</script>
	</body>
	</html>

运行上面的代码我们会发现不论我们怎么单击按钮都无法出现弹出框显示内容为‘double click’

我在面试时遇到这个问题，由于时间有限我想到了用定时器来解决，用一个状态值标志着出发了一次还是两次。现在回想当时的面试感觉回答的还是漏洞百出。

### 解决方案

我在stackflow中找到了这样一个[解决方案](https://stackoverflow.com/questions/5497073/how-to-differentiate-single-click-event-and-double-click-event)

先贴个代码：

	// Author:  Jacek Becela
	// Source:  http://gist.github.com/399624
	// License: MIT

	jQuery.fn.single_double_click = function(single_click_callback, double_click_callback, timeout) {
	  return this.each(function(){
	    var clicks = 0, self = this;
	    jQuery(this).click(function(event){
	      clicks++;
	      if (clicks == 1) {
	        setTimeout(function(){
	          if(clicks == 1) {
	            single_click_callback.call(self, event);
	          } else {
	            double_click_callback.call(self, event);
	          }
	          clicks = 0;
	        }, timeout || 300);
	      }
	    });
	  });
	}

这个代码的实现跟我的思路相同，但是在stackflow中并未找到兼容大部分浏览器的方案，这个代码 在chrome，safari，FF，opera上没多大问题，存在比较明显的问题就是IE，ie<9不支持

访问[http://gist.github.com/399624](http://gist.github.com/399624)有正宗的解释（访问这个链接需要翻墙）

### IE兼容性处理

	$(this).bind("dblclick", function(event) {
        clicks = 2;
        double_click_callback.call(self, event);
    });
    $(this).bind("click", function(event) {
        setTimeout(function() {
            if (clicks != 2) {
                single_click_callback.call(self, event);
            }
            clicks = 0;
        }, timeout || 300);
    });

由于在ie<9中click 和 dbclick的触发判断时间更短，会造成dbclick失效的问题，测试连续出发三次会出现想要dbclick的效果，所以需要将代码改成上面的实现格式。

### 依然存在的问题

根据该连接上提供的解决方案，判断浏览器的类型和版本的代码发现还是有问题，你会发现结果无法执行，【IE兼容性处理】中我们提到过click和dbclick的判断时间很短，用上面的代码判断浏览器的类型和版本存在问题，不能执行，所以提供了如下的解决方案

	(window.ActiveXObject && parseInt(navigator.appVersion.split(";")[1].replace(/[ ]/g, "").replace("MSIE","")) < 9)

根据浏览器的特有对象来判定浏览器的类型是个不错的方法

提供一个[demo]()供大家参考