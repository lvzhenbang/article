# 装饰者设计模式

装饰者设计模式就是装饰器对目标对象添加新的成员（属性、方法）。

注：它符合` 开闭原则 `

## 装饰者模式的实现

```
var obj = {
	name: '<obj>'
}

// 装饰器
obj.printName = function() {
	console.log(this.name)
}

obj.printName()
```

## ES7 ` decorator `

```
let readOnly = function(target, name, descriptor){
	descriptor.writable = false
	return descriptor
}

class Fac {
	@readOnly
	printName() {
		console.log('tfac a.')
	}
}

let fac = new Fac()
fac.printName()
fac.printName = function() {
	console.log('readonly')
}
```