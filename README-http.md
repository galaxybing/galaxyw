- [廖雪峰的官方网站](http://www.liaoxuefeng.com/)
	+ [TCP/IP简介](http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/0014320037768360d53e4e935ca4a1f96eed1c896ad1217000)
		- 互联网的协议 - 互联网协议包含了上百种协议标准，但是最重要的两个协议是TCP和IP协议，所以，大家把互联网的协议简称TCP/IP协议。
		- IP协议
			- IP地址对应的实际上是计算机的网络接口
			- IP地址实际上是一个32位整数（称为IPv4），以字符串表示的IP地址如`192.168.0.1`实际上是把32位整数按8位分组后的数字表示，目的是便于阅读。
			- IP端口 每个网络程序都向操作系统申请唯一的端口号
		- TCP协议 - （Transmission Control Protocol 传输控制协议）
			- TCP协议则是建立在IP协议之上的
		- 更高级的协议: SMTP\telnet\FTP\http
			- 许多常用的更高级的协议都是建立在TCP协议基础上的，比如用于浏览器的HTTP协议、发送邮件的SMTP协议等
			- [Telnet](http://www.baike.com/wiki/telnet)
				* 不过Telnet的主要用途还是使用远程计算机上所拥有的信息资源，如果你的主要目的是在本地计算机与远程计算机之间传递文件，则使用FTP会有效得多
			- SMTP、POP3、IMAP4 常用的电子邮件协议
	+ [HTTP协议简介](http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/001432011939547478fd5482deb47b08716557cc99764e0000)

1. [Hypertext Transfer Protocol (HTTP/1.1)](https://tools.ietf.org/html/rfc7231)
1. [W3C](http://www.w3.org/)
1. [csswg](https://drafts.csswg.org/)
1. [W3C中国（首页右上角语言版本切换，下拉列表）](http://www.chinaw3c.org/)
1. [W3C标准（主导航标签项）](http://www.chinaw3c.org/standards.html)
1. [正式推荐标准（W3C Recommendations）列表](http://www.w3.org/TR/tr-technology-stds)
1. [HTTP > Recommendation](https://www.w3.org/TR/2015/REC-eventsource-20150203/)

***

### TCP连接
- TCP握手 - 3个数据包(SYN、SYN+ACK、ACK)的发送过程
- 测量TCP握手和SSL握手的具体耗时:
	```
	curl -w "TCP handshake: %{time_connect}, SSL handshake: %{time_appconnect}\n" -so /dev/null https://www.alipay.com
	上面命令中的w参数表示指定输出格式，time_connect变量表示TCP握手的耗时，time_appconnect变量表示SSL握手的耗时
	```
- tcp协议中 SYN: Synchronize sequence numbers
	+ SYN: TCP连接的第一个包,非常小的一种数据包, 两个参数来规定tcp内数据包的大小
		- iphdr->tot_len 总长度字段(16位)是指整个IP数据报的长度,以字节为单位
		- tcphdr->doff  TCP头长度，指明了在TCP头部包含多少个32位的字
	+ [SYN Flood](http://blog.csdn.net/zzz_781111/article/details/4183743)
	````
	创建很多的半开放式连接（在S返回一个确认的SYN-ACK包的时候有个潜在的弊端，他可能不会接到C回应的ACK包;S需要耗费一定的数量的系统内存来等待这个未决的连接，虽然这个数量是受限的。）来发动SYN洪水攻击
	````
	+ [第一篇，怎么增加SYN数据包的大小（syn flood攻击实验）](http://blog.csdn.net/wuchunlai_2012/article/details/50379091)
- ACK (ACKnowledge Character）确认字符，解 释: 在数据通信传输中，接收站发给发送站的一种传输控制字符。它表示确认发来的数据已经接受无误。

### http
- HTTPs链接和HTTP链接都建立在TCP协议之上。HTTP链接比较单纯，使用三个握手数据包建立连接之后，就可以发送内容数据了。
- http数据包
	```
	将请求封装成http数据包，然后封装成Tcp数据包，再封装成Ip数据包， 通过物理硬件（网卡芯片）发生到指定地点；
	收到方先发现收到的是个ip数据包，所以通过ip协议解析Ip数据包，然后又发现里面是tcp数据包，就通过tcp协议解析Tcp数据包，接着发现是http数据包通过http协议再解析http数据包得到数据。
	```
- [Headers 使用 view source 来查看](http://www.sina.com.cn/)
	- General
	- Response Headers
	```
	HTTP/1.1 200 OK
	Server: nginx/1.6.1
	Date: Wed, 22 Mar 2017 02:38:34 GMT
	Content-Type: text/html
	Transfer-Encoding: chunked
	Connection: keep-alive
	Vary: Accept-Encoding
	Cache-Control: no-cache, must-revalidate
	Expires: Sat, 26 Jul 1997 05:00:00 GMT
	Pragma: no-cache
	DPOOL_HEADER: dryad30
	SINA-LB: aGEuMjM2LmcxLnF4Zy5sYi5zaW5hbm9kZS5jb20=
	SINA-TS: ZWZjYTk0Y2UgMCAwIDAgNiAzMzEK
	```
		- Cache-Control - 刷新和输入网址后回车的区别：
			+ 强制刷新(ctrl+F5):
				- 不理会缓存协商,全部重新获取.此时请求中的`Cache-Control`值为`no-cache`
			+ 刷新(F5)时:
				- 当点击浏览器上的刷新，客户端发送的请求中的`Cache-Control`均是`max-age=0`
				- max-age<=0 向server 发送 http 请求确认,该资源是否有修改(要求检查cache，再更新cache);有的话返回200 ,无的话返回304.
			+ 输入网址直接回车时:
				- 如果Expired或`Cache-Control`还未过期,则不会返回304状态，因为浏览器已经不用向web服务器发出请求
	- Request Headers
	```
	POST /visitor/record HTTP/1.1
	Host: passport.weibo.com
	Connection: keep-alive
	Content-Length: 6983
	Cache-Control: max-age=0
	Origin: https://passport.weibo.com
	If-Modified-Since: 0
	User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/56.0.2924.87 Safari/537.36
	Content-Type: application/x-www-form-urlencoded
	Accept: */*
	Referer: https://passport.weibo.com/visitor/visitor?from=iframe
	Accept-Encoding: gzip, deflate, br
	Accept-Language: zh-CN,zh;q=0.8,en;q=0.6
	Cookie: tid=jyJ9fEX2s0gAGdxJmFnjEb/C+s8a+jtwwkxEeUU8Ydo=__100; SINAGLOBAL=4603349318490.446.1484199844458; SCF=AthaWemK235tlfhg-NKSyaFG55Kpg8zwzspsTcfq-0yugLrbygZvkNbhofLQ4bafY2Q12grr99it9yA57u_cI3k.; SUHB=0laUCm5CYDIT4M; _s_tentry=www.liaoxuefeng.com; Apache=9890026367803.057.1490148680385; ULV=1490148680392:6:5:1:9890026367803.057.1490148680385:1489746937512; UOR=blog.feehi.com,weibo.com,www.liaoxuefeng.com; SUB=_2AkMvjWyef8NhqwJRmP4Wym7mao1ywgrEieKZ0Z1FJRMxHRl-yj83qlITtRDAEqF5shti77oaQdgljG-Pfa8Q9Q..; SRT=D.QqHBTrs-TbWuSdRtOcYoWr9NUPvrP3bki-sYUqEuW-WpMdbbNsfndZEFNbHi5mYNUCsuTZXqVdoHPGMNMmEEV4RsV-YrTOV3U-EMSFWjT%21Vk4FSYKEmu4Ntl%2AB.vAflW-P9Rc0lR-ykKDvnJqiQVbiRVPBtS%21r3J8sQVqbgVdWiMZ4siOzu4DbmKPWfOqHp5Ei8NmbYT4PMVZbwUFPCA%21M8; SRF=1490150313; SUBP=0033WrSXqPxfM72-Ws9jqgMF55529P9D9WhO2rxSbHIaiEQr9zleBBDb
	```
		- Content-Type 指示响应的内容
			```
			浏览器就是依靠Content-Type来判断响应的内容是网页还是图片，是视频还是音乐。浏览器并不靠URL来判断响应的内容，所以，即使URL是http://example.com/abc.jpg，它也不一定就是图片
			```
		- HTTP响应如果包含body，也是通过\r\n\r\n来分隔的。请再次注意，Body的数据类型由Content-Type头来确定，如果是网页，Body就是文本，如果是图片，Body就是图片的二进制数据。
		- http的Cookie头（服务器给每个Session分配一个唯一的JSESSIONID，并通过Cookie发送给客户端）
			```
			如，Cookie:Hm_lvt_2ccaa20e3ab062b2e2ec2909021a030e=1445487353; ASP.NET_SessionId=eblytbrw4f0gzmtm0u25wdzp; SERVERID=cd01c659bb82eec8f73697c9b07d1a55|1455854047|1455853778
			```
	- Form Data

- #### http 版本之间差异 [深入HTTP理解 2011-08-18 10:21](http://lrysir.iteye.com/blog/1152071)
1. Http0.9：已过时。只接受GET一种请求方法，没有在通讯中指定版本号，且不支持请求头。由于该版本不支持POST方法，因此客户端无法向服务器传递太多信息
2. HTTP/1.0：这是第一个在通讯中指定版本号的HTTP协议版本，至今仍被广泛采用，特别是在代理服务器中。
3. HTTP/1.1：当前版本。持久连接被默认采用，并能很好地配合代理服务器工作。还支持以管道方式在同时发送多个请求，以便降低线路负载，提高传输速度。
	- HTTP/1.1相较于HTTP/1.0协议的区别主要体现在：
		- 缓存处理
		* 带宽优化及网络连接的使用
		* 错误通知的管理
		* 消息在网络中的发送
		* 互联网地址的维护
		* 安全性及完整性
		* 主要区别在于1.1版本允许多个HTTP请求复用一个TCP连接，以加快传输速度
		```
		在Http1.0中 ，访问一个包含有许多图像的网页文件的整个过程包含了多次请求和响应，每次请求和响应都需要建立一个单独的连接，每次连接只是传输一个文档和图像，上一次和下一次请求完全分离，也就是说在性能方面都有很大的影响。

		为了克服Http1.0这种缺陷，HTTP 1.1支持持久连接，在一个TCP连接上可以传送多个HTTP请求和响应，减少了建立和关闭连接的消耗和延迟。
		一个包含有许多图像的网页文件的多个请求和应答可以在一个连接中传输，但每个单独的网页文件的请求和应答仍然需要使用各自的连接。
		HTTP 1.1还允许客户端不用等待上一次请求结果返回，就可以发出下一次请求，但服务器端必须按照接收到客户端请求的先后顺序依次回送响应结果，以保证客户端能够区分出每次请求的响应内容，这样也显著地减少了整个下载过程所需要的时间。
		```
