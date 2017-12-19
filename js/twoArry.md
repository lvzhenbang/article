## 两个数组之间的操作

在实际的开发应用中我们经常会遇到两个数组之间的操作：
合并两个数组的元素；
从一个数组中除去另一个数组中存在的元素；
保留两个数组中共有的元素

### 合并两个数组

	function mergeArray(arr, arr2) {
		return arr.cotact(arr2)
	}
	
扩展1：

如果需要合并两个以上的数组，数字数目未知，但知道大于等于两个，我们可以考虑，ES6中的spread

	mergeArray = (arr, ...args) => args.reduce((acc, v) => (acc.length > 0 ? acc.concat(v) : arr), [])

扩展2：

上述的操作只是将数组进行了拼接，如果要合并一个没有重复元素的数组则可以考虑下面的方法

过滤数组，然后拼接数组

	function unionArray(arr, arr2) {
		return arr2.filter(function(v){
			return arr.indexOf(v) === -1
		}).concat(arr)
	}
	
当然ES6中也有好的原生方法我们可以使用

	unionArray = (arr, arr2) => Array.from(new Set([...arr, ...arr2]))


### 两个数组相减

	function subArray(arr, arr2) {
		return arr.filter(function(v) {
			// includes 存在兼容性问题
			return arr2.indexOf(v) === -1
		})
	}

### 两个数组做&运算
	
	function andArray(arr, arr2) {
		return arr.filter(function(v) {
			// includes 存在兼容性问题
			return arr2.indexOf(v) > -1
		})
	}

持续更新....

欢迎吐槽 :)
