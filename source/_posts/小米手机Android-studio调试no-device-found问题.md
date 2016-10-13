title: 小米手机Android studio调试no device found问题
date: 2016-07-03 23:37:06
tags: [Android开发,Android Studio]
categories: Android开发
---
### 小米手机Android Studio调试No device found问题
***
最近在自学Android开发，今天打算在我的小米手机上调试我的第一个Android应用。结果被坑了，花了好久才找到问题所在。

真机调试是通过USB调试的，但在小米手机上仅仅打开开发者模式和USB调试是不够的，依然会存在连接不到ADB的问题。

最终发现在小米手机上还需要【开启手机端USB调试端口（Diag USB port enable）】，其他品牌的手机应该是不需要的。

以下就是打开USB调试端口的方法：

* 打开手机拨号，输入“\*#\*#717717#\*#\*”，然后提示“Diag USB port enable”这是才真正开启了USB端口调试，相关闭的话再输入一遍就可以了。

* 还要在PC机上安装USB驱动程序（小米官网或是第三方工具）
