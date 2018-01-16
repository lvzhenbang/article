## 策略设计模式

### 什么是策略。

策略就是根据形势的发展而制定的行动方针。

比如说春节快要到了，我们要回家，回家我们就要选择交通工具。怎么回家我们就制定回家的方案。比如说我吧，我们家在河南一个农村，不管是汽车，火车，飞机都没有直达的。我可以选择从北京到郑州乘火车，然后从北京到长葛做长途汽车，然后从长葛到家做短途汽车。当然也可以选择其他方式，这就要根据自己的实际需要，时间不紧，花费又低我一般就选择这个方案。

### 开发中的策略模式使用

策略模式定义：定义一系列的算法，然后把它们一个个的封装起来，这些封装起来的算法可以相互替换。

举一个年终奖金计算的例子，分为A，B，C，D四类，其中A类总经理，B类部门经理，C类项目负责人，D类开发人员。

一般情况，代码实现如下：

```
var calBonus = function (level, salary) {
	if( level === 'A') {
		return salary*2;
	}
	if( level === 'B') {
		return salary*1.5;
	}
	if( level === 'C') {
		return salary*1.0;
	}
	if( level === 'D') {
		return salary*0.75;
	}
}
```

这种代码的实现方式很常见，但是效果不是那么好，在这个函数体内一个条件就是一个算法策略，在实际开发中它们往往实现比较复杂，代码量比较大，一个两个没什么，如果再增加几个或者十几个呢，或者我们在增加100个呢？明显这种实现方式的弹性很差，复用性也比较差。

如果，代码体积较小，分支较少使用条件判断未尝不可，但是如果，条件分支较多且代码体积较大比建议。


### 组合函数实现重构

开发中我们往往会用的一种组合函数重构的方式来实现，代码修改如下：

```
function levelA (salary) {
	return salary*2;
}

function levelB (salary) {
	return salary*1.5;
}

function levelC (salary) {
	return salary*1.0;
}

function levelD (salary) {
	return salary*0.75;
}

var calBonus = function(leveType, salary) {
	if(levelType === 'A') {
		return leveA(salary);
	}
	
	if(levelType === 'B') {
		return leveB(salary);
	}

	if(levelType === 'C') {
		return leveC(salary);
	}

	if(levelType === 'D') {
		return leveD(salary);
	}
}
```

这样修改后的代码确实使用起来，比刚开始好了许多，但是这样的实现方式，还是弹性不足，如果扩展的话回事calBonus函数的体积越来越大。这时我们往往会考虑设计模式。

### 策略模式实现重构

首先，我们需要创建一个策略类组，然后在创建一个奖金类。具体代码实现如下：

```
// A类
function LevelA() {}

LevelA.prototype.cal = function(salary) {
	return salary*2;
}

// B类
function LevelB() {}

LevelB.prototype.cal = function(salary) {
	return salary*1.5;
}

// C类
function LevelC() {}

LevelC.prototype.cal = function(salary) {
	return salary*1;
}

// D类
function LevelD() {}

LevelD.prototype.cal = function(salary) {
	return salary*0.75;
}

// 定义一个奖金类
function Bonus () {
	this.strategy = null;
	this.salary = null;
}
// 奖金strategy
Bonus.prototype.setStrategy = function(strategy) {
	this.strategy = strategy;
}
// 设置salary
Bonus.prototype.setSalary = function(salary) {
	this.salary = salary;
}
// 获取奖金
Bonus.prototype.getBonus = function() {
	return this.strategy.cals(this.salary)
}

// A
var bonus = new Bonus();
bonus.setSalary(10000);
bonus(new LevelA());
bonus.getBonus()

// B
var bonus = new Bonus();
bonus.setSalary(8000);
bonus(new LevelB());
bonus.getBonus()
```

在javascript中，我们都知道，函数也是对象，所以更简单的方法就是将每一种策略定义成函数，而非上面的实现方式。

### JavaScript策略模式

```
function levelA (salary) {
	return salary*2;
}

function levelB (salary) {
	return salary*1.5;
}

function levelC (salary) {
	return salary*1.0;
}

function levelD (salary) {
	return salary*0.75;
}

var Bonus = {
	levelA : levelA,
	levelB : levelB,
	levelC : levelC,
	levelD : levelD,
};

var calBonus = function(level, salary) {
	return Bonus[level](salary);
}

calBonus('levelA', 30000); // 60000

// 添加一个E类
var levelE = function (salary) {
	return salary*0.5;
}

Bonus.levelE = levelE;

calBonus('levelE', 5000); // 2500
```

这种实现方式，研究源码的同学经常遇到，在jQuery，validator等库和前端框架中比较常见。
