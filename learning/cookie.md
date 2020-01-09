# cookie

## cookie特点

* 明文传输
* 存储在浏览器端

## cookie安全

* HttPOnly属性可以避免js` document.cookie `获取cookie值
* SameSite属性可以避免面XSRF(网站请求伪造)攻击
* 明文传输带来安全隐患
