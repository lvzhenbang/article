# HTTP2

为了方便管理和压缩，HTTP2使用了一个有状态的` HPACK `算法，需要客户端和服务器各自维护一份`索引表`（read-only）。

它废除了HTTP的起始行概念，它把HTTP起始行里的请求方法、URI、状态码等统一转换成了头字段，这些头字段被称为`伪头字段`，格式为` :key: value `，比如：`:method: POST`。

注：原HTTP的` version `和` reason `概念被废除了。

## HTTP2特点

* 压缩头部字段信息，避免‘头重脚轻’的现象，使用[` HPACK `](https://www.zcfy.cc/article/1969)算法进行压缩
* 服务器和客户端传送的是二进制数据（报文头部），因此数据可读性比较差
* HTTP2是应用层协议，所以它只是从应用层上解绝了[` 队头阻塞 `](https://cloud.tencent.com/developer/article/1509279)（因为它允许乱序的requse -> response，相对于HTTP1.x来说他的确解决了）。但是` 对头阻塞 `是在TCP上发生的，所以是在传输层（` 丢包重传 `机制）上并没有解决。
* 大多数浏览器使用HTTP2需要使用TLS

注：使用[` QUIC `](https://cloud.tencent.com/developer/article/1385937)（传输层协议，基于UDP协议）可以解决丢包重传的问题。这样可以从根本上解决` 对头阻塞 `。HTTP3本身实现了` QUIC `（传输层使用的UDP协议）。

注：HTTP2是在HTTP1.x的基础上使用了` HPACK + stream + TLS1.2 `；相对于HTTPs来说，HTTP2需要` SSL/TLS `改为` TLS1.2 `，然后使用了` HTPACK + stream `
