## 掌握Javascript面试：什么是闭包？

文章来源于:[https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-closure-b2f0d2152b36](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-closure-b2f0d2152b36)

在JavaScript的面试中我通常将这个问题放在第一个或者最后一个问题。坦白地说，如果你没有深入的学习闭包你的JavaScript不可能有很深的造诣。

你可能JavaScript稍好点儿，但你真的理解如何构建一个好的JavaScript应用吗?你真的理解什么正在运行，或者应用如何工作的吗？我对此表示怀疑。不知道这个问题的答案在面试的过程中是一个危险的信号。

你不仅应该知道闭包的工作机制是什么，你还应该知道他为什么发生，并且你应该很轻松的回答几种常用的闭包用例。

闭包常用在JavaScript对象的数据私有化，事件句柄，回调函数，和[在局部应用，柯里化](https://medium.com/javascript-scene/curry-or-partial-application-8150044c78b8#.l4b6l1i3x)，以及其他功能的变成形式。

我不在乎面试候选人是否知道‘closure’这个词语或者技术定义。我想要弄明白的是他们是否理解基本的运行机制。如果他们不知道，显而易见这些面试候选者并没有大量的实际JavaScript应用开发经验。

> “如果你不能回答这个问题，你就是一个初级开发人员。我不管你工作了几年。”

这听起来意味着什么，但实际上并不是。我的意思是大多数称职的面试官会问你什么是闭包，并且在大多数时候你回答错误将失去这份工作。如果你足够幸运的话，你将得到一个offer，他们将给你一个初级开发人员的工资而不是一个高级的开发人员。

准备好快速跟进：“你能说出两种常用的闭包吗？”

### 什么是闭包？

闭包就是一个函数(封闭的)的集合引用的环境（词法环境）状态。换句话说，闭包有能力从一个内部函数访问外部函数的作用域。在JavaScript中，在函数被创建时，每次一个函数被创建闭包也被创建。

用一个闭包，只需在一个函数内部定义一个函数，暴露这个内部的函数，然后返回这个函数，或者把它传递给另一个函数。内部的函数将有能力访问外部函数作用域的变量，即使外部的函数有返回值

### 使用闭包（实例）

刨除其他的，闭包通常用于对象的数据私有化。数据的私有化是帮助我们开发接口的一个重要的属性，而不是实现（应用的开发的细节实现）。他是一个帮助我们开发一个稳健的软件的重要概念，因为实现细节往往比接口约定更容易被打破。

> “程序之于接口，而不是实现”

[设计模式：可重用的面向对象软件的元素](http://www.amazon.com/gp/product/B000SEIBB8?ie=UTF8&camp=213733&creative=393177&creativeASIN=B000SEIBB8&linkCode=shr&tag=eejs-20&linkId=CSQYBHTUP625XI4T)

在javascript中闭包是的主要机制就是被用来实现数据的私有化。当你用笔包进行数据的私有化，所包含的变量仅被包含在在外部的函数作用域内。除了通过对象的特权方法外，你将不能从外部范围获取数据。在闭包的范围内定义的任何公开方法都是特权的。例如：
	
	const getSecret = (secret) => {
	  return {
	    get: () => secret
	  };
	};

	test('Closure for object privacy.', assert => {
	  const msg = '.get() should have access to the closure.';
	  const expected = 1;
	  const obj = getSecret(1);

	  const actual = obj.get();

	  try {
	    assert.ok(secret, 'This throws an error.');
	  } catch (e) {
	    assert.ok(true, `The secret var is only available
	      to privileged methods.`);
	  }

	  assert.equal(actual, expected, msg);
	  assert.end();
	});

在上面的例子中，‘.get()’方法是在‘getsecret()’范围内定义的，这使得它可以从‘getsecret()’访问任何变量,并使它 成为一个特权方法。

使对象的数据私有化并不是闭包的唯一用途。它也可以用来创建有状态的函数，这些函数的返回值可能受他们内部状态的影响。示例如下：

	const secret = msg => () => msg;

	// Secret - creates closures with secret messages.
	// https://gist.github.com/ericelliott/f6a87bc41de31562d0f9
	// https://jsbin.com/hitusu/edit?html,js,output

	// secret(msg: String) => getSecret() => msg: String
	const secret = (msg) => () => msg;

	test('secret', assert => {
	  const msg = 'secret() should return a function that returns the passed secret.';

	  const theSecret = 'Closures are easy.';
	  const mySecret = secret(theSecret);

	  const actual = mySecret();
	  const expected = theSecret;

	  assert.equal(actual, expected, msg);
	  assert.end();
	});

在函数式编程中，闭包常常被用在局部应用&柯里化编程,这里需要明白一些定义：

应用程序：应用一个函数的参数已返回一个值得过程

部分应用：函数应用他的部分参数的过程，这个部分被应用的函数稍后被用来获得返回值。换句话来说，一个函数转变一个多参数的函数，并利用它的返回一个少参数的函数。

部分应用利用闭包的作用域来处理参数对象，你可以写一个泛型函数部分的将参数应用于目标函数，下面有一个示例：

	partialApply(targetFunction: Function, ...fixedArgs: Any[]) =>
  		functionWithFewerParams(...remainingArgs: Any[])

它将接受一个带有任意数量参数的函数，接下来我们想要部分的应用函数的参数然后返回一个带有剩余参数的函数。

下面一个例子，一个求两个数字和的函数：
	
	const add = (a, b) => a + b;

现在你想要一个实现对任意数字都加10的函数，我们命名它为‘add10()’。‘add10(5)’的结果应该是‘15’,我们顶一个‘partialAply()’的函数，如下：
	
	const add10 = partialApply(add, 10);
	add10(5);

在这个例子中，参数‘10’变成了一个固定的参数被保存在‘add10()’的闭包作用域中。
下面是‘partialApply()’的实现代码：
	
	// Generic Partial Application Function
	// https://jsbin.com/biyupu/edit?html,js,output
	// https://gist.github.com/ericelliott/f0a8fd662111ea2f569e

	// partialApply(targetFunction: Function, ...fixedArgs: Any[]) =>
	//   functionWithFewerParams(...remainingArgs: Any[])
	const partialApply = (fn, ...fixedArgs) => {
	  return function (...remainingArgs) {
	    return fn.apply(this, fixedArgs.concat(remainingArgs));
	  };
	};


	test('add10', assert => {
	  const msg = 'partialApply() should partially apply functions'

	  const add = (a, b) => a + b;

	  const add10 = partialApply(add, 10);


	  const actual = add10(5);
	  const expected = 15;

	  assert.equal(actual, expected, msg);
	});

正如我们从上面的示例中看到的，这个简单的返回函数可以访问‘fixArgs’参数，这个参数是从‘partialApply()’中传入的。