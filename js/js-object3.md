## 对象的属性

在JavaScript中对象的属性分为两种：数据属性和访问器属性

### 数据属性

数据属性包含了一个数据值的位置。在这个位置可以读取和写入值。它有四个特性：

* configurable：可删除操作。默认值为true。
* enumerable: 可循环（for...in）。默认值为true。
* writable：可修改。默认值为true。
* value：该属性的数据值。默认值为undefined。

熟悉ES5的Object.defineProperty()的同学应该不陌生。该方法可以用来设置对象的属性。它接收三个参数，第一个对象，第二个为对象的属性名，第三个为一个描述对象，这个描述对象的属性即为属性的四个特性。

注意：一旦将对象的属性特性configurable设定为false后，属性的特性除了writable外，都将不能再修改。

IE8是第一个支持Object.defineProperty()的浏览器，但它存在限制，只能在DOM对象上使用该方法，而且只能创建访问器属性。

### 访问器属性

访问器属性不包括数据值。读取访问器属性用getter；写入指访问器属性用setter。

访问器属性也有四个特性：

* configurable：可删除操作。默认值为true。
* enumerable: 可循环（for...in）。默认值为true。
* get：读取访问器属性。默认值为undefined。
* set：写入访问器属性。默认值为undefined。

注意：对象属性前加下划线，如：`_name`。表示只能通过对象的方法访问的属性。

### 定义对象多个属性

```
var person = {}

Object.defineProperties(person, {
	name: {
		value: 'lin'
	},
	_age: {
		configuralbe: true,
		writable: true,
		value: 20
	},
	age: {
		get: function() {
			return this._age
		},
		set: function(n) {
			this._age = n > 20 ? n : this._age
		}
	}
})

```

前两个为数据属性，第三个为访问器属性。

注意：使用Object.defineProperties()方法定义多个属性（原对象中没有该属性，利用Object.defineProperties()添加并定义新的属性。Object.defineProperty效果相似），如果没有定义属性的相关特性，该特性将默认为false。

### 数据属性VS访问器属性

数据属性特性是对该属性本的定义（configurable：是否可删除；enumerable：可枚举；writable，可修改；value: 该属性属性值的存储位置）。

访问器属性是对属性访问方式的处理，它可以让使用者自定义属性的读取和写入。

像上面的示例代码中 访问器属性`age` 是对 `_age` 数据属性的访问方式的重新定义。