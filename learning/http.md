# HTTP

## HTTP 特点

* 简单、灵活、易于扩展

结构简单，报文的格式就是`header + body`。因为简单，所以就灵活；因为灵活，所以就容易扩展

* 应用广泛、环境成熟

实现了`跨语言、跨平台`地使用

* 无状态

好处是减少了数据的传送量；更容易集群，实现负载均衡，把请求转发到任意一台服务器；也方便了把多台服务器组合起来，从而实现高并发

坏处是因为服务器没有记忆能力，所以它无法支持连续、多步骤的`事务`操作。例如购物过程。

注：cookie，redis可以解决无状态的问题

* 明文（不安全）

传输的报文未加密，不具有隐秘性，易受黑客篡改；没有完整的校验，无法证明访问者的身份

注：HTTPs改善了这一点

* 性能不算好，也不算坏

web常用的有缓存，切图、资源合并与压缩、CDN、Javascript的特殊用法。

## HTTP实体数据

* conttent-type：指定文件的[` 类型（MIME） `](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types); 也可以指定字符的编码类型[` charset `](hhttps://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Accept-Charset)
* content-encoding：文件的[` 压缩方式 `](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Accept-Encoding)
* content-language：指定文件内容是哪种[` 语言 `](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Language)

## 参考资料

* [` HTTP权威指南总结 `](https://github.com/woai30231/http) 

注：总结的还算不错，不足就是语句不通顺。如果英文水平还行推荐看《HTTP权威指南》这本书。

* [` HTTP2讲解 `](https://legacy.gitbook.com/book/ye11ow/http2-explained/details)

注： `HTTP2详解`推荐看[` 英文 `](https://daniel.haxx.se/http2/)

* [` HTTP 接口设计指北 `](https://github.com/bolasblack/http-api-guide)
