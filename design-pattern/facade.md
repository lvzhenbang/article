## 外观设计模式

当我们竖起一个门面时，我们向外展示的只是一种外表，它可能隐藏着一个非同一般的事实。这也就是我们所要说的外观设计模式，这种模式为一大段的代码体提供了一个便捷的高级接口，隐藏了底层复杂的实现。这种设计模式简化代码的呈现形式，给开发人员一个API，同时也能提高代码的可用性。

外观设计模式在JavaScript的库中很常见，例如JQuery，尽管实现的是可能支持具有广泛行为的方法（如：$('selector')用来获取selector所指代的对象），但展示给使用者的只是一个 `facade` 或者这些方法的有限抽象。

我们经常正面接触的是外观（Facade），而不是外观背后具体实现的子系统。就如我们用jQuery，不管什么时候都是用的$(el).css()，$(el).animate()，我们使用的都是用的简单的公共接口，这样避免我们手动调用jQuery core的内部方法，也避免了手动操作与dom的交互和维护状态变量的需要。

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

外观设计模式虽然少有劣势，但值得注意的一个问题是性能。

抽象出一个外观设计模式都要花费一些抽象成本，这样我们就要靠这个成本的花费是否合理。

以Jquery库中的获取元素为例，原生的getElementById('identifer')要比$('#identifer')的速率要快的多，但是我们要记住jquery的Sizzle选择器引擎在幕后做了很多的优化查询，同时他返回一个jquery对象，而不是返回一个dom节点。

这个特殊的外观设计模式为了实现一个优雅的选择器函数，它能接受和解析多种类型的查询，但它隐含着抽象的成本。时间证明jquery的Sizzle引擎是成功的。实际上一个简单的外观设计模式为一个团队来说是很有必要的，在使用模式时注意所涉及的性能成本，对它们是否值得提供抽象调用做评估。

