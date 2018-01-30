
### 使用注意问题

1.两个对象数据的比较

```
var obj = {
	a:1,
	b:2,
	c:3
}

var obj2 = {
	a:1,
	b:2,
	c:3
}

function compare(origin, target) {
	// 比较两个对象的属性长度
	if(Object.keys(origin).length !== Object.keys(target).length) return false
	// 判断两个对象的类型，如果有一个是对像，另一个不是，则返回false，
	// 如果两个都是都不是按照非对象类型比较方式
    if (typeof target === 'object') {
        if (typeof origin !== 'object') return false
        // 遍历比较对象的属性
        Object.keys(target).forEach(function(key) {
        	// 进行递归
            if (!compare(origin[key], target[key])) return false
        })
        return true
    } else {
    	return origin === target
    }
}
```

对于两个未知的对象我们使用递归来进行深度遍历比较是否存在不相等的属性，如果存在返回false；反之，则返回true。

我们再看看JavaScript开发中的其它常见数据类型的比较方式。

变量的比较我们通过 `===` 或 `!==` 来判断两个对象是否相等。

```
var a = 1;
var b = 1;
console.log(a === b); // true
```

两个数组中数字元素的比较

```
var a = [1, 2, 3, 4, 5];
var b = [1, 3, 5, 7, 9];
console.log(a[0] === b[0]); // true
console.log(a[1] === b[1]); // false
```

连个数组中的字符串元素比较

结果同上

两个数组中的对象元素的比较

```
var arr = [{a:1}, {b:2}, {c:3}, {d:4}, {e:5}];
var arr2 = [{a:1}, {c:3}, {e:5}];

console.log(arr[0] === arr2[0]); // false
console.log(arr[] === arr2[1]); // false
```

这里的问题很明显就是对象数据类型的比较用了一般字符，数字变量数据类型的比较方式。

在JavaScript中一般的字符串数组，数字数组，字符串，数字，我们都可以用一个变量指向他们的具体值，而对象我们只能指向他们的索引，而不能只想具体的值，所以，会出现 arr[0] 不等于 arr2[0]的情况，因为它们的指针不同，所以对于对象我们要比较它们的非对象属性，如果对象的属性也是对象就是用递归方法。

这一点，初学者往往会自动过滤掉，特别是数组的元素是对象的情况。