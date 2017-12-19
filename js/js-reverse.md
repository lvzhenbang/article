## js中的反转

js中的反转主要有以下三种，数字反转，字符串反转，数组的反转

### 数组的反转

	var arr = [1,2,3,4,5];
	arr = arr.reverse();
	console.log(arr);

	结果: [5,4,3,2,1]

### 字符串的反转

先将字符串转换为数组，然后反转数组，最后将数组转合并为字符串

	var str = 'hello world';
	str = str.split('').reduce(function(acc, v) {
		return v + acc
	}, '');
	console.log(str);

	结果：'dlrow olleh'

> 兼容性：

由于reduce()存在兼容性问题，>ie8的浏览器可以很好的使用，但是ie8是个问题。
	
	var str = 'hello world';
	str = str.split('').reverse().join('');
	console.log(str);

既然有这么有这个兼容性更好的方法,为什么还要用reduce(),我想聪明的你应该很清楚那就是，运行的效率，前者的效率更高。我在自己电脑上跑的实例对比图如下：

![reverse](https://github.com/lvzhenbang/article/blob/master/img/js/img-reverse.png)

### 数字的反转

数字的反转说起来也很简单，就是将数字转化为字符串然后按照字符串的处理方式处理

	var num = 123456789;
	num = num.toString().split('').reduce(function(acc, v)) {
		return v + acc
	}, '');

	console.log(num);
	结果：987654321

### 针对以上3种情况的一个综合解决方案

	function objReverse(obj) {
		if (Object.prototype.toString.call(obj) === '[object String]') {
			obj = obj.split('');
			return stringReverse(obj);
		} else if (Object.prototype.toString.call(obj) === '[object Number]') {
			obj = obj.toString().split('');
			return +stringReverse(obj);
		} else if (Object.prototype.toString.call(obj) === '[object Array]') {
			return obj.reverse();
		}

		function stringReverse (obj) {
			if(Array.prototype.reduce !== 'undefined') {
				return obj.reduce(function(acc, v) {
					return v + acc;
				}, '')	
			} else {
				return obj.reverse().join('');
			}
		}
	}

欢迎吐槽 :)