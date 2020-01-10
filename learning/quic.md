# QUIC

[` QUIC `]是基于UDP协议的。因为UDP协议可以接受无序的数据包，所以可以从根本上解决` 对头阻塞 `。

和TCP协议相比UDP协议很简单，它只是对应用层的数据进行了简单的封装，随意这是一个不可靠的协议。

正因为UDP协议简单，甚至简单到不需要建连和断连，所以它的` 扩展性 `很强。

因此` QUIC `选中了UDP。

因为UDP不具备`连接管理、拥塞窗口、流量控制`等功能，所以` QUIC `本身实现了这些功能。

注：` QUIC `最早是Google发明的，被称为[` gQUIC `](https://www.chromium.org/quic)

## QUIC的特点

* 因为它基于UDP，所以不需要` 握手、挥手 `操作，所以数据传输速度更快
* 它比弥补了UDP传输不可靠的缺点，同时引入了HTTP2的` 流 `和` 多路复用 `的特性（不同于HTTP2，它不仅支持双向流，也支持单向流）
* 它支持了[` TLS1.3 `](http://blog.nsfocus.net/tls1-3/)

注：` QUIC `并不是基于` TLS1.3 `，而是它只处理的是TLS的` 记录 `，而握手信息和警报信息都不适用TLS` 记录 `。

注：` QUIC `相对于TCP的三次握手，再加上` TLS1.2 `的至少两次安全握手来说，它使用` TLS1.3 `的最多一次握手，再加之本身不需要握手，所以` 连接建立延时 `更低

## 其他学习资料

* [1]. https://www.chromium.org/quic
* [2]. https://docs.google.com/document/d/1gY9-YNDNAB1eip-RTPbqphgySwSNSDHLq9D5Bty4FSU/edit
* [3]. E. Rescorla, “The Transport Layer Security (TLS) Protocol Version 1.3”, draft-ietf-tls-tls13-21, https://tools.ietf.org/html/draft-ietf-tls-tls13-21, July 03, 2017
* [4]. Adam Langley,Wan-Teh Chang, “QUIC Crypto”，https://docs.google.com/document/d/1g5nIXAIkN_Y-7XJW5K45IblHd_L2f5LTaDUDwvZ5L6g/edit, 20161206
* [5]. https://www.multipath-tcp.org/
* [6]. Ha, S., Rhee, I., and L. Xu, "CUBIC: A New TCP-Friendly High-Speed TCP Variant", ACM SIGOPS Operating System Review , 2008.
* [7]. M. Belshe,BitGo, R. Peon, “Hypertext Transfer Protocol Version 2 (HTTP/2)”, RFC 7540, May 2015
* [8]. M. Mathis,J. Mahdavi,S. Floyd,A. Romanow,“TCP Selective Acknowledgment Options”, rfc2018, https://tools.ietf.org/html/rfc2018, October 199
* [9]. V. Paxson，M. Allman，J. Chu，M. Sargent，“Computing TCP's Retransmission Timer”， rfc6298, https://tools.ietf.org/html/rfc6298, June 2011
* [10]. R. Peon,H. Ruellan,“HPACK: Header Compression for HTTP/2”,RFC7541,May 2015
* [11]. M. Scharf, Alcatel-Lucent Bell Labs, S. Kiesel, “Quantifying Head-of-Line Blocking in TCP and SCTP”, https://tools.ietf.org/id/draft-scharf-tcpm-reordering-00.html, July 15, 2013
* [12]. Ilya Grigorik，“Optimizing TLS Record Size & Buffering Latency”, https://www.igvita.com/2013/10/24/optimizing-tls-record-size-and-buffering-latency/, October 24, 2013
* [13]. J. Salowey,H. Zhou,P. Eronen,H. Tschofenig, “Transport Layer Security (TLS) Session Resumption without Server-Side State”, RFC5077, January 2008
* [14]. Dierks, T. and E. Rescorla, "The Transport Layer Security (TLS) Protocol Version 1.2", RFC 5246, DOI 10.17487/RFC5246, August 2008, http://www.rfc-editor.org/info/rfc5246.
* [15]. Shirey, R., "Internet Security Glossary, Version 2", FYI , RFC 4949, August 2007
* [16]. 罗成，“HTTPS性能优化”， http://www.infoq.com/cn/presentations/performance-optimization-of-https，February.2017
* [17]. Postel, J., "Transmission Control Protocol", STD 7, RFC793, September 1981.
* [18]. J. Postel,“User Datagram Protocol”, RFC768,August 1980
* [19]. Q. Dang, S. Santesson,K. Moriarty,D. Brown.T. Polk, “Internet X.509 Public Key Infrastructure: Additional Algorithms and Identifiers for DSA and ECDSA”,RFC5758, January 2010
* [20]. Bassham, L., Polk, W., and R. Housley, "Algorithms and Identifiers for the Internet X.509 Public Key Infrastructure Certificate and Certificate Revocation List (CRL) Profile", RFC 3279, April 2002
* [21]. D.Cooper,S.Santesson, S.Farrell,S. Boeyen,R. Housley,W.Polk, “Internet X.509 Public Key Infrastructure Certificate and Certificate Revocation List (CRL) Profile”, RFC5280, May 2008
* [22]. M. Allman,V. Paxson,E. Blanton, "TCP Congestion Control”,RFC5681, September 2009
* [23]. Robbie Shade, “Flow control in QUIC”, https://docs.google.com/document/d/1F2YfdDXKpy20WVKJueEf4abn_LVZHhMUMS5gX6Pgjl4/edit#, May, 2016,
* [24]. ianswett , “QUIC fec v1”, https://docs.google.com/document/d/1Hg1SaLEl6T4rEU9j-isovCo8VEjjnuCPTcLNJewj7Nk/edit#heading=h.xgjl2srtytjt, 2016-02-19
* [25]. D.Borman,B.Braden,V.Jacobson,R.Scheffenegger, Ed. “TCP Extensions for High Performance”,rfc7323, https://tools.ietf.org/html/rfc7323,September 2014
* [26]. 罗成，“WEB加速，协议先行”， https://zhuanlan.zhihu.com/p/27938635，july, 2017
