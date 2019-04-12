title: Ubuntu Shadowsocks全局代理
date: 2017-01-02 13:57:19
tags: [运维, 代理]
categories: 运维
---
　　我一般用Ubuntu的桌面系统，在使用过程中经常需要到国外的服务器下载某些东西，例如Android Studio、Eclipse等。因此需要科学上网，u know ^^
　　我是使用Shadowsocks搭建的代理服务，使用的是socks5协议,而终端很多工具目前只支持http和https等协议，对socks5协议支持不够好，所以我们为终端设置shadowsocks的思路就是将socks协议转换成http协议.
　　想要进行转换，需要借助工具，这里我们采用比较知名的polipo来实现。polipo是一个轻量级的缓存web代理程序。
　　假设我们已经搭建好Shadowsocks代理，我们讲一下polipo的安装与配置：
<!--more-->
### 　安装
``` bash
sudo apt-get install polipo
```
###　修改配置
　　打开配置文件
``` bash
sudo vim /etc/polipo/config
```
　　设置ParentProxy为Shadowsocks
``` bash
# Uncomment this if you want to use a parent SOCKS proxy:

socksParentProxy = "localhost:1080"
socksProxyType = socks5
```
　　设置日志输出文件
``` bash
logFile=/var/log/polipo
logLevel=4
```
###　启动
``` bash
sudo service polipo stop
sudo service polipo start
```
###　使用
　　打开System Settings->Network->Network proxy->Manual，修改配置为：
``` bash
HTTP Proxy 127.0.0.1:8123
HTTPS Proxy 127.0.0.1:8123
FTP Proxy 127.0.0.1:8123
```
　　8123是polipo默认的端口
