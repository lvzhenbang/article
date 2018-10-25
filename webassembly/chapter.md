## WebAssembly

### 概览

随着web技术的发展，JavaScript在web开发中占的比重越来越重，但随之而来的问题也来了，而最受人诟病的确是下面两个问题：

* 语法太灵活
* 性能不能满足一些场景的需要

所以，针对JavaScript的问题，各大浏览器生产商提出了不同的方案：

微软的 [TypeScript](http://www.typescriptlang.org/) 引入了静态类型检测来解决JavaScript的松散语法，从而提升代码的健壮性；

谷歌的 [Dart](https://www.dartlang.org/) 引入了一个虚拟机，可以在上面运行Dart程序，这个程序可以提升应用的性能；

火狐的 [asm.js](http://asmjs.org/) 是JavaScript的一个子集，浏览器的JavaScript引擎针对 `asm.js` 做了性能优化。

虽然三大浏览器生产商都提出了自己的解决方案，但是它们互不兼容，解决的也是上面两个问题中的一个。

于是 `WebAssembly` 诞生了。

### WebAssembly的原理

`WebAssembly` 是一种新的字节码格式，而非是一种编程语言。因为 `WebAssembly` 字节码和底层机器码很相似，不像js那样需要解释执行，所以 `WebAssembly` 的性能很好。而 `WebAssembly` 执行的是字节码，所以需要一个虚拟机来执行字节码，浏览器生产商则是根据 `WebAssembly` 的规范实现虚拟机。由于字节码写起来不方便，所以需要高级语言实现虚拟机能执行的字节码。这样的高级语言有很多，但是能在浏览器中运行的只有JS，所以只能用JS去加载和执行 `WebAssembly` 。

### JS 调用 WebAssembly

JS调用 `WebAssembly` 分为3步：

1. 加载字节码（浏览器可以通过http请求，Node.js可以通过 `require()` 来读取字节码文件）
2. 编译字节码（可以使用WebAssembly.compile编译，然后通过 `Promise.resolve` 向下传递一个module）
3. 实例化（用WebAssembly.Instance实例化这个被传递下来的module）

实际开发中，使用 `WebAssembly.instantiate` 来实现编译和实例化这两个步骤，代码如下：

```
WebAssembly.instantiate(bytes, {
    window: {
        alert: window.alert
    }
}).then(module => module.instance.f(50))
```

### WebAssembly能干些什么

`WebAssembly` 的出现主要是为了解决JavaScript的性能瓶颈问题，针对于需要大量计算的场景它是一个很好的解决方案。

在web应用中比较难搞的就是音频与视频，我们所熟悉的 [`flv.js`](https://github.com/Bilibili/flv.js/) 如果用WebAssembly重写后性能就有了很大的提升；

此外react的 dom diff中设计大量计算，使用WebAssembly重写的话，性能也会有较大提升；

由于Safari浏览器使用的JS引擎也已经支持 `WebAssembly` ，所以RN应用使用它性能会有很大的提升；

另外，就是解决了大型3D网页游戏性能

### WebAssembly 局限性

1. 它是一个新的技术规范，只有最新版本的浏览器支持，此外是各大浏览器间的实现不同；
2. 学习资料比较少；
3. 生态不完善；
4. 有待进一步的实践，挖掘它潜在的问题

[WebAssembly JavaScript API](https://developer.mozilla.org/zh-CN/docs/WebAssembly/Using_the_JavaScript_API)