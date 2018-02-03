### Async/await

async作为解决异步回调的未来解决方案，现在已经被广泛使用了，虽然它存在着兼容性问题，但是好在我们有了babel这样的语法转换神器，它可以将async语法转换为大多数浏览器支持的ES5代码。

作为一个node爱好者，一直在关注着web框架express和koa，koa作为epxress的未来解决方案，它已经支持了async。

注意：如果你的node < 7.6，那么要在koa中使用async就要引入babel。同时，要想正确解析和编译async，我们要引入transform-aasync-to-generator。如果，大家有兴趣的话，可以看 `[https://koa.bootcss.com/](https://koa.bootcss.com/)` 官方文档。

下面是是一个实例代码：

```
const Koa = require('koa');
const app = new Koa();

app.use(async ctx => {
  ctx.body = 'Hello koa.';
});

app.listen(3000);
```
