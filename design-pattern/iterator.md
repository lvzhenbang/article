## iterator 设计模式

迭代器模式就是按照顺序访问一个对象中元素，而不用暴露该对象的内部组成。迭代器模式就是将这个迭代实现从业务中分离出来。

但实际开发中我们并不将他当成一个设计模式。

### 前瞻后顾

说起迭代器，想必对ES6有了解的同学应该不会陌生。我们知道，`for ... of` 遍历的对象必须是迭代器对象，而普通对象则不能，因为普通对象内部没有实现迭代器，而像数组则内部实现了迭代器，所以可以用`for ... of` 的语法，而对于一般对象在ES5中有专门的处理方法，`for ... in` 和 
`Object.keys()` ，而 `for ... in` 可遍历所有的的对象，但是它遍历特殊对象，如数组，也会遍历它的length，这并不是我们需要的，有时还会出现不按顺序的遍历。

在我们日常使用中一般是将普通对象转化为特殊对象然后处理的。

### 仿jQuery迭代器

这里我只简单的实现数组的遍历，至于如何迭代普通对象，我们下面再做介绍。

```
var $ = {
	each: function (arr, fn) {
		for (var i = 0, len = arr.length; i < len; i++) {
			fn.call(arr[i], i, arr[i])
		}
	}
};

$.each([1, 2, 3, 4, 5, 6], function(i, val) {
	console.log([i, val]);
});
```

### 迭代器的分类

迭代器根据实现的位置，我们将它分为内部迭代器和外部迭代器两种。

### 内部迭代器

内部迭代器对于使用者来说他不用关心迭代器的内部实现，只用关注使用的效果，我们上面仿jQuery的each就是个内部迭代器的实现。

内部迭代器有它的好处但是也有它的不足，比如我们要比较两个数组是否相等，上面的方法就不满足我们的需要，我们就需要写一个新的方法来实现。

```
var $ = {
	each: function (arr, fn) {
		for (var i = 0, len = arr.length; i < len; i++) {
			fn.call(arr[i], i, arr[i])
		}
	}
};

var compareArray = function(arr, arr2) {
	if( arr.length !== arr2.length) {
		return false;
	}
 
	$.each(arr, function(i, val) {
		if( val !== arr2[i]) {
			return false;
		}
	});
	return true;
};

compareArray([1, 2], [1, 2, 3]); // false

```

### 外部迭代器

外部迭代器必须显式地请求才会迭代下一个元素。

外部迭代器虽然增加了使用上的一些麻烦，但是它的灵活性却正是我们需要的。我们可以人为的控制迭代的过程和顺序。

```
// 迭代器实现
var Iterator = function(obj) {
	var current = 0;
 
    var next = function() {
    	current += 1;
    };
 
    var isDone = function() {
    	return current >= obj.length;
    };
 
    var getCurrItem = function() {
    	return obj[current];
    };

    var len = function() {
    	return obj.length;
    }
 
    return {
    	next: next,
    	isDone: isDone,
    	getCurrItem: getCurrItem,
    	length: len,
    }
};
// 比较数组
var compareArray = function (iteratorObj, iteratorObj2) {
	if(iteratorObj.length !== iteratorObj2.length) {
		return false;
	}
	while (!iteratorObj.isDone() && !iteratorObj2.isDone()){
		if (iteratorObj.getCurrItem() !== iteratorObj2.getCurrItem()){
			return false;
		}
		iteratorObj.next();
		iteratorObj2.next();
	}
 
    return true;
};

var arr = Iterator([1, 2, 3]); 
var arr2 = Iterator([1, 2, 3]); 
 
compareArray(arr, arr2); // true
```

这样我们就用ES5实现了迭代器的功能，ES6的实现迭代器相对简单，如果不熟悉的可以参考一下阮一峰老师的 [ES6 使用手册](http://es6.ruanyifeng.com/#docs/iterator)

### 迭代对象

使用ES6迭代器后发现，`for ... of` 能够遍历的迭代器对象，如: 数组，类数组，Set，Map，arguments等对象它们有一个共同的特性，就是它们都有一个length数组，可以实现对对象用下标进行访问。

因此，要实现对普通对象的的迭代，我们可以参考jQuery的实现如下做：

```
var isArraylike = function(obj) {
	return Object.prototype.toString.call(obj) === [object Array];
}
$ = {
	each: function(obj, fn) {
	    var isArray = isArraylike(obj); // 判断对象是否为数组

	    if (isArray) {
	    	for (var i = 0, len = obj.length ; i < len; i++ ) {
	            if (fn.call(obj[i], i, obj[i]) === false) {
	            	break;
	            }
	        }
	    } else {
	    	for (i in obj) {
	    		if (fn.call(obj[i], i, obj[i]) === false) {
	    			break;
	    		}
	    	}
	    }
	}
}; 

```

我们再使用ES6处理一般对象时一般使用两种方法，一种是将普通对象转化为迭代器对象，另一种就是上面这种写法。

### 终止迭代器

通过上面的例子，我们发现迭代器可以实现像普通for循环中使用break一样跳出循环。

```
if (fn.call(obj[i], i, obj[i]) === false) {
	break;
}
```

它表示回调函数返回fasle后，那么循环终止。

```
var $ = {
	each: function(arr, fn) {
		for(var i = 0, len = arr.length; i < len; i++) {
			if(fn(i, arr[i]) === false) {
				break;
			}
		}
	}
};

$.each([1, 2, 3, 4, 5], function(i, val) {
	if(val > 3) return false;
	console.log(val);
})
```

其实，按照分类还有一个倒序迭代器，由于比较简单这里就不多说，你可以自己实现以下，其实，就是封装一个 `reverse()` 。
