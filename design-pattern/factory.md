## factory 设计模式

### 定义

工厂模式是另一个有关创建对象概念的模式。

和其他的设计模式的区别？

在于它没有显示地要求我们使用构造函数，相反，它为创建对象提供一个通用的接口，用这个接口，我们可以创建我们想创建的指定类型的工厂对象。

注：它将构造函数和创建者分离，符合` 开放封闭原则 `。

### 实例说明

假如我们有一个UI工厂，我们被要求创建一个UI组件类型。我们不用心操作符创建这个组件，也不是通过另一个creational 构造函数创建这个组件，而是向工厂对象请求一个新的组件。我们通知工厂我们需要什么类型的对象，例如：‘button’，‘panel’，它会实例化它，然后返回给我们使用。

### 使用场景

如果创建的对象比较复杂，这个就比较实用。

例如：对象强烈的依赖于动态因素或应用程序配置。

这种模式的的例子在ExtJS这样的UI库总很常见，在那里，创建对象或组件的方法可以进一步划分。

#### 代码示例

示例用构造函数模式来定义汽车，它演示了如何使用工厂模式实现车辆工厂：

```
// Types.js - 在以后的场景中需要用到的构造函数

// 定义新汽车的构造函数
function Car( options ) {
	// 默认参数
	this.doors = options.doors || 4;
	this.state = options.state || "brand new";
	this.color = options.color || "silver";
}
	
// 定义新卡车的构造函数
function Truck( options){
	this.state = options.state || "used";
	this.wheelSize = options.wheelSize || "large";
	this.color = options.color || "blue";
}
	
	
// FactoryExample.js
	
// 定义一个车辆工厂骨架
function VehicleFactory() {}
	
// 为工厂定义 prototypes 和 utilities
	
// 默认 vehicleClass 是 Car
VehicleFactory.prototype.vehicleClass = Car;
	
// 创建新 vehicle 实例的工厂方法
VehicleFactory.prototype.createVehicle = function ( options ) {
	switch(options.vehicleType){
		case "car":
			this.vehicleClass = Car;
			break;
		case "truck":
			this.vehicleClass = Truck;
			break;
		// 默认是 VehicleFactory.prototype.vehicleClass (Car)
	}
	return new this.vehicleClass( options );
};
	
// 创建一个 car 类的工厂实例
var carFactory = new VehicleFactory();
var car = carFactory.createVehicle( {
	vehicleType: "car",
	color: "yellow",
	doors: 6
} );
	
// 测试确认我们的 car 用 vehicleClass/prototype Car 创建的
	
// 输出: true
console.log( car instanceof Car );
	
// 输出: Car object of color "yellow", doors: 6 in a "brand new" state
console.log( car );
```

接下来，修改 VehicleFacory 的实例使用卡车(truck)类

```
var movingTruck = carFactory.createVehicle( {
	vehicleType: "truck",
	state: "like new",
	color: "red",
	wheelSize: "small"
	} );
	
// 测试确认我们的 truck 用 vehicleClass/prototype Truck 创建的
	
// 输出: true
console.log( movingTruck instanceof Truck );
	
// 输出: Truck object of color "red", a "like new" state
// and a "small" wheelSize
console.log( movingTruck );
```

我们用 VehicleFactory 子类创建一个 truck 工厂类，

```
function TruckFactory () {}
TruckFactory.prototype = new VehicleFactory();
TruckFactory.prototype.vehicleClass = Truck;
	
var truckFactory = new TruckFactory();
var myBigTruck = truckFactory.createVehicle( {
	state: "omg..so bad.",
	color: "pink",
	wheelSize: "so big"
} );
	
// 确认 myBigTruck 由 prototype Truck 创建的
// 输出: true
console.log( myBigTruck instanceof Truck );
	
// 输出: Truck object with the color "pink", wheelSize "so big"
// and state "omg. so bad"
console.log( myBigTruck );
```

### 什么时候用工厂模式

有以下情况出现时工厂模式会很好用：

1. 当我们的对象或组件的设置有一很高的复杂度时；
2. 当我们需要根据所处的环境生成不同的对象实例时；
3. 当我们处理很多小对象或组件时，它们共享相同的属性
4. 当用其他对象的实例组合对象时，只需要满足API的约定（aka， duck typing）就可以工作了。这对于解耦很有用

### 什么时候不用工厂模式

当应用到作物类型的问题时，这种设计模式会给程序带来不必要的复杂性。除非为对象创建提供接口是我们正在编写的库或框架的设计目标，否则建议继续使用显示构造函数来避免不必要的开销。

由于对象创建的过程时在一个接口后面进行抽象的，所以这也可能导致单元测试的问题，这取决于这个过程可能有多复杂。

### 抽象工厂模式

了解抽象的工厂模式也是很有用的，它的目标是将一组具有共同目标的单个工厂封装起来。它将一组对象的实现细节与它们的一般用法分离开来。

抽象的工厂模式应该被使用在系统必须独立于它创建的对象的方式情况下，或者它需要处理多种类型的对象。

紧接上面的例子，VehicleFactory它定义了获取和注册车辆类型的方法。抽象的工厂可以命名AbstractVehicleFactory，它允许对车辆的类型进行定义，如：car，truck，具体的工厂将只实现满足车辆协议的Vehicle.prototype.drive 和 Vehicle.prototype.breakDown。

```
var abstractVehicleFactory = (function () {

	// 存储车辆的类型
	var types = {};
	
	return {
		getVehicle: function ( type, customizations ) {
			var Vehicle = types[type];

			return (Vehicle ? new Vehicle(customizations) : null);
		},

		registerVehicle: function ( type, Vehicle ) {
			var proto = Vehicle.prototype;

			// 仅注册有 vehicle contract 的
			if ( proto.drive && proto.breakDown ) {
				types[type] = Vehicle;
			}

			return abstractVehicleFactory;
		}
	};
})();
	
	
// Usage:
	
abstractVehicleFactory.registerVehicle( "car", Car );
abstractVehicleFactory.registerVehicle( "truck", Truck );
	
// Instantiate a new car based on the abstract vehicle type
var car = abstractVehicleFactory.getVehicle( "car", {
	color: "lime green",
	state: "like new"
} );
	
// Instantiate a new truck in a similar manner
var truck = abstractVehicleFactory.getVehicle( "truck", {
	wheelSize: "medium",
	color: "neon yellow"
} );
```
