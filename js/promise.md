## 深入理解promise

对于现在的前端同学来说你不同promise你都不好意思出门了。对于前端同学来说promise已经成为了我们的必备技能。

那么，下面我们就来说一说promise是什么，它能帮助我们解决什么问题，我们应该如何使用它？

这是我个人对promise的理解。欢迎吐槽 ：)

### Promise是什么

promise的意思是承诺，有的人翻译为许愿，但它们代表的都是未实现的东西，等待我们接下来去实现。

Promise最早出现在commnjs，随后形成了Promise/A规范。在Promise这个技术中它本身代表以目前还不能使用的对象，但可以在将来的某个时间点被调用。使用Promise我们可以用同步的方式写异步代码。其实Promise在实际的应用中往往起到代理的作用。例如，我们像我们发出请求调用服务器数据，由于网络延时原因，我们此时无法调用到数据，我们可以接着执行其它任务，等到将来某个时间节点服务器响应数据到达客户端，我们即可使用promise自带的一个回调函数来处理数据。

### Promise能帮我们解决什么痛点

JavaScript实现异步执行，在Promise未出现前，我们通常是使用嵌套的回调函数来解决的。但是使用回调函数来解决异步问题，简单还好说，但是如果问题比较复杂，我们将会面临回调金字塔的问题（pyramid of Doom）。

```
var a = function() {
    console.log('a');
};

var b = function() {
    console.log('b');
};

var c = function() {
    for(var i=0;i<100;i++){
        console.log('c')
    }  
};

a(b(c)); // 100个c -> b -> a
```
我们要桉顺序的执行a，b，c三个函数，我们发现嵌套回调函数确实可以实现异步操作（在c函数中循环100次，发现确实是先输出100个c，然后在输出b，最后是a）。但是你发现没这种实现可读性极差，如果是几十上百且回调函数异常复杂，那么代码维护起来将更加麻烦。

那么，接下来我们看一下使用promise（promise的实例可以传入两个参数表示两个状态的回调函数，第一个是resolve，必选参数；第二个是reject，可选参数）的方便之处。

```
var promise = new Promise(function(resolve, reject){
    console.log('............');
    resolve(); // 这是promise的一个机制，只有promise实例的状态变为resolved，才会会触发then回调函数
});

promise.then(function(){
    for(var i=0;i<100;i++) {
        console.log('c')
    }    
})
.then(function(){
    console.log('b')
})
.then(function(){
    console.log('a')
})
```

那么，为什么嵌套的回调函数这种JavaScript自带实现异步机制不招人喜欢呢，因为它的可读性差，可维护性差；另一方面就是我们熟悉了jQuery的链式调用。所以，相比起来我们会更喜欢Promise的风格。

### promise的3种状态

上面提到了promise的 `resolved` 状态，那么，我们就来说一下promise的3种状态，未完成（unfulfilled）、完成（fulfilled）、失败（failed）。

在promise中我们使用resolved代表fulfilled，使用rejected表示fail。

#### ES6的Promise有哪些特性

1. promise的状态只能从 `未完成->完成`, `未完成->失败` 且状态不可逆转。

2. promise的异步结果，只能在完成状态时才能返回，而且我们在开发中是根据结果来选择来选择状态的，然后根据状态来选择是否执行then()。

3. 实例化的Promise内部会立即执行，then方法中的异步回调函数会在脚本中所有同步任务完成时才会执行。因此，promise的异步回调结果最后输出。示例代码如下：

```
var promise = new Promise(function(resolve, reject) {
  console.log('Promise instance');
  resolve();
});

promise.then(function() {
  console.log('resolved result');
});
for(var i=0;i<100;i++) {
console.log(i);
/*
Promise instance
1
2
3
...
99
100
resolved result
*/
```
上面的代码执行输出结果的先后顺序，曾经有人拿到这样一个面试题问过我，所以，这个问题还是要注意的。

#### resolve中可以接受另一个promise实例

resolve中接受另一个另一个对象的实例后，resolve本实例的返回状态将会有被传入的promise的返回状态来取代。

reject状态替换实例，代码如下：

```
const p1 = new Promise(function (resolve, reject) {
    cosole.log('2秒之后，调用返回p1的reject给p2');
    setTimeout(reject, 3000, new Error('fail'))
})

const p2 = new Promise(function (resolve, reject) {
    cosole.log('1秒之后，调用p1');
    setTimeout(() => resolve(p1), 1000)
})

p2
  .then(result => console.log(result))
  .catch(error => console.log(error))

// fail
```
resolve状态替换实例，代码如下：

```
const p1 = new Promise(function (resolve, reject) {
    cosole.log('2秒之后，调用返回p1的resolve给p2');
    setTimeout(resolve, 3000, 'success')
})

const p2 = new Promise(function (resolve, reject) {
    cosole.log('1秒之后，调用p1');
    setTimeout(() => resolve(p1), 1000)
})

p2
  .then(result => console.log(result))
  .catch(error => console.log(error))

// success
```
注意：promise实例内部的resolve也执行的是异步回调，所以不管resolve放的位置靠前还是靠后，都要等内部的同步函数执行完毕，才会执行resolve异步回调。

```
new Promise((resolve, reject) => {
    console.log(1);
    resolve(2);
    console.log(3);
}).then(result => {
    console.log(result);
});
/*
1
3
2
*/
```

这个问题也在面试题中出现过，所以，要牢记。

### promise和ajax如何结合使用

```
function PromiseGet (url) {
    return new Promise( (resolve, reject) => {
        let xhr = new XMLHttpRequest()
        xhr.open('GET', url, true)
        xhr.onreadystatechange = function () {
            if (this.readyState === 4) {
                if (this.status === 200) {
                    resolve(this.responseText, this)
                } else {
                    let resJson = {
                        code: this.status,
                        response: this.response
                    }
                    reject(resJson, this)
                }
            }
        }
        xhr.send()
    })
}
```

我们发现用promise技术结合ajax，只是在promise实例中引入ajax，在ajax请求处理的结果中使用了resolve和reject状态。

前面我们说了resolve()，返回执行then()代表完成，那么，reject()代表失败，返回执行catch()，同时运行中抛出的错误也会执行catch()

现在有一个非常好用的promise和Ajax结合的github项目[Axios](https://github.com/axios/axios) ，想要深入了解的同学可以研究下它的源码。

### promise.all()方法可以处理一个以promise实例为元素的数组

let promise = Promise.all([p1, p2, p3])

promise 的状态由p1，p2，p3共同决定。当它们都为resolve状态时，promise状态为true，它们的返回值组成一个数组，传递给promise；它们只要有一个的状态为reject，就将该实例的返回值传递给promise

### promise.race()方法也可以处理一个promise实例数组

但它和promise.all()不同，从字面意思上理解就是竞速，那么理解起来上就简单多了，也就是说在数组中的元素实例那个率先改变状态，就向下传递谁的状态和异步结果。

### 将一个普通对象转化为Promise对象

在开发中我们经常会遇到$.ajax()的使用且会遇到$.ajax()间的依赖使用，由于两个或多个$.ajax()间是同步的，如果我们并排着写实现不了依赖关系，所以，我们往往使用嵌套，但是对于ajax这样复杂的结构，嵌套不是个好办法，我们需要先将代码抽象提取，做一下封装，代码如下：

```
/*
url：地址
data：数据，在函数内部会转化成json。如果没传，表示用GET方法；如果传了，表示用POST方法
*/
function ajax(url, data, callback) {
    $.ajax({
      url: url,
      type: data == null ? 'GET' : 'POST',
      dataType: "json",
      data: data == null ? '' : JSON.stringify(data),
      async: true,
      contentType: "application/json",
      success: function (data) {
          callback(data);
      },
      error: function (XMLHttpRequest, textStatus) {
        if (XMLHttpRequest.status == "401") {
            window.parent.location = '...';
            self.location = '...';
        } else {
            alert(XMLHttpRequest.responseText);
        }
      }
    });
}
```

那么，我们应该如何避免回调金字塔呢？很显然，我们可以将它与promise结合，代码如下：

```
function ajax(url, data, callback) =>
    new Promise((resolve, reject) => {
        $.ajax({
            url: url,
            type: data == null ? 'GET' : 'POST',
            dataType: "json",
            data: data == null ? '' : JSON.stringify(data),
            async: true,
            contentType: "application/json",
            success: function (data) {
                callback(data);
                resolve();
            },
            error: function (XMLHttpRequest, textStatus) {
                if (XMLHttpRequest.status == "401") {
                    window.parent.location = '...'
                    self.location = '...'
                } else {
                    alert(XMLHttpRequest.responseText);
                }
                reject()
            }
        })
    })  
}
```
当然，这是不熟悉jQuery的同学，或者考虑长线Promise的，但是jQuery也为我们提供了按顺序调用多个$.ajax()的方案，那就是deferred，它模拟了promise的实现，有兴趣的同学可以查看[源码](https://github.com/jquery/jquery/blob/master/src/deferred.js)，看它是如何实现的。实例代码如下：

```
$.ajax({
    url:'./a'
}).then(function(){
    return $.ajax({ url:'./b' });
}).then(function(){
    return $.ajax({ url:'./c' });
}).then(function(){
    return $.ajax({ url:'./d' });
}).then(function(){
    //TODO here
});
```
### promise存在的问题

* promise一旦执行，无法中途取消
* promise的错误无法在外部被捕捉到，只能在内部进行预判处理
* promise的内如何执行，监测起来很难

正是因为这些原因，ES7引入了更加灵活多变的async，await来处理异步。

这个稍后，后续可能还会继续修改，也欢迎各位批评指正。有问题或者有其他想法的可以在我的[GitHub](https://github.com/lvzhenbang/article)上pr。
