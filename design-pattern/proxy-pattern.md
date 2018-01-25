## 代理模式

所谓的的代理模式就是为一个对象找一个替代对象，以便对原对象进行访问。

使用代理的原因是我们不愿意或者不想对原对象进行直接操作，我们使用代理就是让它帮原对象进行一系列的操作，等这些东西做完后告诉原对象就行了。就像我们生活的那些明星的助理经纪人一样。

我们举一个明星买鞋子的例子。

1.明星自己去买鞋。

```
// 定义一个鞋子类
var Shoes = function(name){
	this.name = name;
};

Shoes.prototype.getName = function() {
	return this.name;
};

// 定义一个明星对象
var star = {
	buyShoes: function(shoes) {
		console.log('买到了一双' + shoes.getName());
	}
}

star.buyShoes(new Shoes('皮鞋')); // "买到了一双皮鞋"
```

当然了，想买鞋这种事，一般都会交给助理去做。

2.明星让助理代自己去买鞋。

```
// 定义一个鞋子类
var Shoes = function(name){
	this.name = name;
};

Shoes.prototype.getName = function() {
	return this.name;
};

// 定义一个助理对象
 
var assistant = {
	buyShoes: function(shoes) {
		star.buyShoes(shoes.getName())
	}
};

// 定义一个明星对象
var star = {
	buyShoes: function(name) {
		console.log('买到了一双' + name);
	}

};

assistant.buyShoes(new Shoes('高跟鞋')); // "买到了一双高跟鞋"
```

怎么样，代理就是这么简单，可能到这里有的同学会比较疑惑，代理的实现结果不是和不使用一样吗？是的，一样的实现结果是必须的，但是，值用代理并不是我们看到的那样将简单的事情复杂化了，代理的使用场景当然不是这种简单的场景，而是针对一些比较复杂或特殊的情况使用，这里只是为了举例说明代理的实现。下面就介绍一些使用场景。

### 代理使用场景

继续上面的明星买鞋子的问题。在生活中我们会遇到商店在营业时间，而你在工作时间，由于要挣钱同时又要花钱，所以，会找一个代理；就像春节快到了，你没时间或者抢不到票，就会找票贩子一样；像现在的代购，则是你不能出国，或者对国外不了解，就找能出国，对国外了解的人帮你买东西一样。我们知道每家商店都有自己的营业时间和休息时间，这里我们用（8:00~20:00）算作营业时间。

```
// 定义一个鞋子类
var Shoes = function(name){
	this.name = name;
};

Shoes.prototype.getName = function() {
	return this.name;
};
// 添加了一个business方法，通过当前的时间来判断是否能买到鞋子。
Shoes.prototype.business = function() {
	var curTime = new Date().getHours();
	return  curTime >= 8 && curTime <= 20 ? that.getName() : '"非营业时间！"';
	
}

// 定义一个助理对象
var assistant = {
	buyShoes: function(shoes) {
		star.buyShoes(shoes.getName())
	}
};

// 定义一个明星对象
var star = {
	buyShoes: function(name) {
		console.log('买到了一双' + name);
	}
};

assistant.buyShoes(new Shoes('高跟鞋')); // "买到了一双高跟鞋"
```

### 保护代理

助理作为明星的代理，不仅可以帮助明星买东西，同时还有帮助明星过滤的东西的职责，比如说，有粉丝要送明星花（不是什么样的花都收的），有人要找明星代言广告（不是什么样的广告都代言的）。

```
// 定义一个广告类
var Ad = function(price){
	this.price = price;
};

Ad.prototype.getPrice = function() {
	return this.price;
};

// 定义一个助理对象
var assistant = {
	init: function(ad) {
		var money = ad.getPrice();
		if(money > 300) {
			this.receiveAd(money);
		} else {
			this.rejectAd();
		}
	},
	receiveAd: function(price) {
		star.receiveAd(price);
	},
	rejectAd: function() {
		star.rejectAd();
	}
};

// 定义一个明星对象
var star = {
	receiveAd: function(price) {
		console.log('广告费' + price + '万元');
	},
	rejectAd: function() {
		console.log('拒绝小制作！');
	}
};

assistant.init(new Ad(5)); // "拒绝小制作！"
assistant.init(new Ad(500)); // "广告费500万元"
```

像这种明星向助理授权，如：什么样价位的广告可以接，什么样的鲜花可以接等等。这样将一些业务的处理交给助理或者经纪人处理，而自己则位于幕后，无疑给自己减少了不必要的麻烦，这样明星就处于一种保护状态。在现实生活中的例子比比皆是，同样在我们的程序语言开发中也是比较常见，尤其是网络和进程这方面，相信做过nodjs开发的同学或多或少会遇到。

### 虚拟代理

在开发中，我们往往将 `new Ad('5')` 这个对象的实例化操作，放到函数内部执行，这样的操作会减少不必要的实例化对象的开销，造成资源的浪费。这种使用的情况我们将之成为虚拟代理。

下面就介绍一个常见的虚拟代理——图片的预加载。

图片预加载是一种常见的前端技术，由于图片过大或者网络不佳，我们不会直接给某个img标签节点设置src属性，而是使用一张loading图片作为占位符，然后用异步的方式来家在加载图片，等到图片加载完毕，我们再把它填充到img的节点里。

```
var preImage = (function() {
	var imgNode = document.createElement('img');
	document.body.appendChild(imgNode);
	var img = new Image; 
    img.onload = function() {
    	imgNode.src = img.src;
    }; 
 
    return {
    	setSrc: function(src) {
    		imgNode.src = '../loading.gif';
    		img.src = src;
    	}
    }
})(); 
 
preImage.setSrc('https://cn.bing.com/az/hprichbg/rb/TadamiTrain_ZH-CN13495442975_1920x1080.jpg'); 
```

到这里，图片的预加载功能已经实现，但是往往体现一段代码的是否更优秀，是看你的代码是否易于维护，特别是对于海量的代码。第一点，这段代码不符合单一职责，我们把负责图片预加载的功能，对img元素的处理放在了一个函数体内，尤其是没有将代码变化的部分和未变化的部分分开；第二点，就是将来我们的网速非常好，我们不用再担心由于网络不佳而造成的显示效果问题，那么关于图片预加载的功能就要去掉。

我们可以尝试使用代理来实现，代码如下：

```
var myImage = (function() {
	var imgNode = document.createElement('img');
	document.body.appendChild(imgNode);
	return {
    	setSrc: function(src) {
    		imgNode.src = src;
    	}
    }
})();

var preImage = (function() {
	var img = new Image; 
    img.onload = function() {
    	myImage.setSrc = img.src;
    }; 
 
    return {
    	setSrc: function(src) {
    		myImage.setSrc('../loading.gif');
    		img.src = src;
    	}
    }
})(); 
 
preImage.setSrc('https://cn.bing.com/az/hprichbg/rb/TadamiTrain_ZH-CN13495442975_1920x1080.jpg'); 
```

这样我们就将图片预加载和为img元素节点设置src分开来。

### 代理和被代理对象的一致性

因为代理要实现和被代理对象实际处理一样的效果，所以，在实现代理对象时，原对象有的方法，代理对象一样有，这样可以保证，用户在操作代理对象时就像在操作原对象一样。

### 缓存代理

缓存代理就是将代理加缓存，下面是一个求和的例子：

```
var multAdd = function() {
	var res = 0;
	for (var i = 0, l = arguments.length; i < l; i++) {
		res = res + arguments[i]
	}

	return res;
};

var proxyAdd = (function() {
	var cache = {};
	return function() {
		var args = Array.prototype.join.call(arguments, ',');
		if(args in cache) {
			return cache[args];
		}
		return caches[args] = multAdd.apply(this, arguments);
	}
})();

proxyAdd(1, 2, 3); // 6
proxyAdd(1, 2, 3); // 6
```

我们不用代理当然也能实现缓存就和，但是为了达到单一职责，我们可以让multAdd实现求和，而缓存则放在代理中来实现。


当然，还有其他的分类代理，比如，智能代理，远程代理。但是在JavaScript中我们使用最多，也最常见的就是虚拟代理和缓存代理。