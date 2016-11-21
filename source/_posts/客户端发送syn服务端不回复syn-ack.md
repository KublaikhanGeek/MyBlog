title: 客户端发送syn服务端不回复syn ack
date: 2016-10-26 16:02:14
tags: [TCP/IP,网络]
categories: TCP/IP
---
### 客户端发送syn服务端不回复syn ack
***
在搭建一个服务的时候，公司内部测试都可以，但是在客户那里就一直无法访问服务器，试了多部手机都不行，最后发现客户那里安卓手机都无法访问服务，ios手机可以正常访问服务(安卓是linux内核默认开启了时间戳，ios是BSD默认没有开启时间戳)。

<!--more-->

那只好抓包看了，然后在客户端和服务端两端进行抓包，对比发现客户端在一直发送syn,但是服务端一直没有回复syn ack，客户端一直在重发syn。也就是说客户端一直在尝试和服务端建立链接，却一直被服务器丢掉。

可以通过以下命令：

    netstat -s |grep timestamp

发现服务器拒绝了不少包，都是因为时间戳的问题。

 RFC1323 中有如下一段描述：

> An additional mechanism could be added to the TCP, a per-hostcache of the last timestamp received from any connection.This value could then be used in the PAWS mechanism to rejectold duplicate segments from earlier incarnations of theconnection, if the timestamp clock can be guaranteed to haveticked at least once since the old connection was open. Thiswould require that the TIME-WAIT delay plus the RTT togethermust be at least one tick of the sender’s timestamp clock.Such an extension is not part of the proposal of this RFC.

 大概意思是说TCP有一种行为，可以缓存每个连接最新的时间戳，后续请求中如果时间戳小于缓存的时间戳，即视为无效，相应的数据包会被丢弃。 

  tcp_tw_recycle/tcp_timestamps都开启的条件下，60s内同一源ip主机的socket connect请求中的timestamp必须是递增的。

最终暂时在服务端关闭了net.ipv4.tcp_timestamps参数，解决了这个问题（最终是什么导致的时间戳乱序还是没有找到）

还需要详细了解一下systcl.conf下相关的参数。

可以参考一下链接：
[http://www.cnxct.com/coping-with-the-tcp-time_wait-state-on-busy-linux-servers-in-chinese-and-dont-enable-tcp_tw_recycle/](http://www.cnxct.com/coping-with-the-tcp-time_wait-state-on-busy-linux-servers-in-chinese-and-dont-enable-tcp_tw_recycle/)
[http://www.litrin.net/2013/03/01/android%E4%B9%8B%E7%BD%91%E7%BB%9C%E4%B8%A2%E5%8C%85%E4%BA%8B%E4%BB%B6/](http://www.litrin.net/2013/03/01/android%E4%B9%8B%E7%BD%91%E7%BB%9C%E4%B8%A2%E5%8C%85%E4%BA%8B%E4%BB%B6/)

