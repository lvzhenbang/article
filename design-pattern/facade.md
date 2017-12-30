## 外观设计模式


当我们提出一种表象时，我们向世界呈现一种外表，这可能掩盖了一种完全不同的现实。这就是我们要回顾的模式背后的灵感——Facade模式。此模式为更大的代码体提供了一个方便的高级接口，隐藏了其真实的底层复杂性。把它看作是为其他开发人员将API简化，这几乎总能提高可用性。

外观设计模式在JavaScript的库中很常见，例如JQuery，尽管实现的是可能支持具有广泛行为的方法（如：$('selector')用来获取selector所指代的对象），但展示给使用者的只是一个 `facade` 或者这些方法的有限抽象。

我们经常正面接触的是外观（Facade），而不是外观背后具体实现的子系统。就如我们用jQuery，不管什么时候都是用的 `$(el).css()`，`$(el).animate()`，我们使用的都是用的简单的公共接口，这样避免我们手动调用jQuery core的内部方法，也避免了手动操作与dom的交互和维护状态变量的需要。

jQuery core的方法被视为中间抽象。开发人员直接接触的是DOM API和让jQuery变得好用的外观设计模式的应用（Facade pattern）

基于我们所了解到的，外观设计模式简化了一个类接口，并弱化类的使用。不直接于子系统进行交互可以减少错误的出现率。

外观设计模式的优点：

1.易于使用

2.在实现形式上通常占用较小的内存。

这里有一个未经优化的例子，它能实现监听事件的跨浏览器使用，这种情况我们一般是通过创建一个通用的方法来实现，在方法中我们检查被使用的属性是否存在，以便提供安全且能跨浏览器的解决方案。

	var addMyEvent = function( el, ev, fn ){
 
	   if( el.addEventListener ) {
	            el.addEventListener( ev, fn, false );
	      } else if(el.attachEvent) {
	            el.attachEvent( "on" + ev, fn );
	      } else {
	           el["on" + ev] = fn;
	    }
	 
	};

在jQuery中我们找到了一个相似的实现 `$(document).ready(...)`，而其内部的实现是通过调用 `bindReady()` 方法来实现的，如下是其内部实现：

	bindReady: function() {
	    ...
	    if ( document.addEventListener ) {
	      // Use the handy event callback
	      document.addEventListener( "DOMContentLoaded", DOMContentLoaded, false );
	 
	      // A fallback to window.onload, that will always work
	      window.addEventListener( "load", jQuery.ready, false );
	 
	    // IE 情况下
	    } else if ( document.attachEvent ) {
	 
	      document.attachEvent( "onreadystatechange", DOMContentLoaded );
	 
	      window.attachEvent( "onload", jQuery.ready );
	               ...

用户只使用这个被暴露出来的 `$(document).ready(...)` 即可，其他的复杂实现，被隐藏了起来。

外观设计模式不一定单独使用，它可能和其他设计模式一起使用，例如：Module设计模式，正如我们下面看到的，我们的模块设计模式的实例包含了一些私有的方法。通过外观设计模式可以用一些更简单的API访问这些方法。

	var module = (function() {
 
	    var _private = {
	        i: 5,
	        get: function() {
	            console.log( "current value:" + this.i);
	        },
	        set: function( val ) {
	            this.i = val;
	        },
	        run: function() {
	            console.log( "running" );
	        },
	        jump: function(){
	            console.log( "jumping" );
	        }
	    };
	 
	    return {
	 
	        facade: function( args ) {
	            _private.set(args.val);
	            _private.get();
	            if ( args.run ) {
	                _private.run();
	            }
	        }
	    };
	}());
	 
	 
	// usage
	module.facade( {run: true, val: 10} );
	// Outputs: "current value: 10" and "running"

在这个示例中，调用 `module.facade()` 将最终最终出发一个私有方法 `set` ，使用者不关心这个，他只只要调用就可以了，而不用担心实现的具体细节。

### 抽象的注意事项

外观设计模式通常没有什么缺点，但值得注意的是性能。也就是说，有人必须确定抽象的抽象是否为我们的实现提供了隐性成本，如果是这样的话，这个成本是否合理。回到jQuery库，我们大多数人都知道getElementById("identifier")和$("# identifier")都可以通过ID来查询页面上的元素。

但是，你知道getElementById()在其自身上的速度要比它快得多吗?看看这个jsPerf测试看结果在每个浏览器层面:http://jsperf.com/getelementbyid-vs-jquery-id。当然，我们必须记住，jQuery(Sizzle——选择器引擎)在幕后做了更多的工作来优化我们的查询(而且还会返回jQuery对象，而不只是返回一个DOM节点)。

这个特殊的Facade的优势在于，为了提供一个优雅的选择器函数，能够接受和解析多种类型的查询，有一种隐含的抽象成本。用户不需要访问jQuery.getById("identifier")或jQuery.getByClass("identifier")等。这就是说，性能上的权衡在实践中已经经过了多年的实践，并且由于jQuery的成功，一个简单的外观实际上对团队来说非常有效。

在使用该模式时，请注意所涉及的任何性能成本，并对它们是否值得提供的抽象级别进行调用。
