title: OpenWRT开发入门
date: 2016-11-21 18:51:25
tags: [OpenWRT]
categories: OpenWRT
---
本文主要讲解如何编译自己的ipk包到OpenWRT系统上。
为了让自己的程序能跑在OpenWRT系统上，我们可以有两种选择：
>1. 配置交叉编译环境，通过toolchain（工具链）进行交叉编译，得到在OpenWRT系统上运行的二进制文件。
2. 利用SDK来生成ipk文件。

接下来我们主要讲一下第二种方法。
<!--more-->
SDK就是将编译好的文件再打包到一个ipk中，便于安装。SDK与toolchain一样，可以自己编译也可以从网上下载。SDK生成ipk包时需要的只是一个makefile文件，makefile里写了所需下载的文件、生成规则、软件版本、类型等。makefile的写法可以到openwrt的官方wiki中找到。
  
我们以生成helloworld包为例子。进入到OpenWRT的SDK目录下的package目录，在该目录下创建我们的helloworld目录，并创建Makefile(这是OpenWRT中生成包用的，不是编译包源码的Makefile)，同时将源码拷贝到helloworld目录下，形成下面的目录结构：
``` bash
package
|-- helloworld
|   |-- Makefile
|   `-- src
|       |-- helloworld.c
|       `-- Makefile
`-- Makefile
```
我们要编写的是 package/helloworld/Makefile 这个文件。

在这个文件中，我们要描述 helloworld 包的信息，比如：如何配置、如何编译、如何打包、安装等等信息。
这个文件与一般的 Makefile 格式还不一样，详见OpenWrt上的说明文档：[OpenWrt官网Packages说明](http://wiki.openwrt.org/doc/devel/packages)

这里我就依照例子编写 helloworld/Makefile：
``` bash
include $(TOPDIR)/rules.mk

PKG_NAME:=helloworld
PKG_RELEASE:=1

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)

include $(INCLUDE_DIR)/package.mk

define Package/helloworld
    SECTION:=utils
    CATEGORY:=Utilities
    TITLE:=Helloworld -- prints a snarky message
endef

define Package/helloworld/description
    It's my first package demo.
endef

define Build/Prepare   #当你的代码是本地的代码，而不是从代码仓库下载时这一步必须有
    echo "Here is Package/Prepare"
    mkdir -p $(PKG_BUILD_DIR)
    $(CP) ./src/* $(PKG_BUILD_DIR)/
endef

define Package/helloworld/install
    echo "Here is Package/install"
    $(INSTALL_DIR) $(1)/bin
    $(INSTALL_BIN) $(PKG_BUILD_DIR)/helloworld $(1)/bin/
endef

$(eval $(call BuildPackage,helloworld))   #已去除逗号后面的空格
```

然后在OpenWRT SDK目录下执行 **make menuconfig** 选中你要创建的包，保存并退出，执行　**make V=99** 就会生成ipk包了。

## 简单讲解一下创建包用的Makefile
***

#### 一、自定义宏
##### 1. 必须定义的宏

我定要为我们的package定义特定的宏：
* Package/<包名>    #包的参数

在 Package/<包名> 宏中定义与包相关的信息。如Package/helloworld宏
``` bash
define Package/helloworld
    SECTION:=utils
    CATEGORY:=Utilities
    TITLE:=Helloworld -- prints a snarky message
endef
```
除了上面所列的 SECTION，CATEGORY，TITLE变量外，还可以定义：
* SECTION     - 包的种类
* CATEGORY    - 显示在menuconfig的哪个目录下
* ITLE       -  简单的介绍
* DESCRIPTION - (deprecated) 对包详细的介绍
* URL - 源码所在的网络路径
* MAINTAINER  - (required for new packages) 维护者是谁（出错了联系谁）
* DEPENDS     - (optional) 需要依事的包，See below for the syntax.
* USERID      - (optional) a username:groupname pair to create at package installation time.

##### 2. 可选定义的宏
其它的宏可以选择性地定义，通常没必要自己重写。但有些情况，package.mk中默认的宏不能满足我们的需求。这时，我们就需要自己重定义宏。

比如，我们在为helloworld写Makefile时，我们要求在编译之前，将 SDK/package/helloworld/src/ 路径下的文件复制到 PKG_BUILD_DIR 所指定的目录下。

于是我们重新定义Build/Prepare宏：
``` bash
define Build/Prepare
    mkdir -p $(PKG_BUILD_DIR)
    $(CP) ./src/* $(PKG_BUILD_DIR)/
endef
```
如此以来，在我们 make V=s 时，make工具会在编译之前执行 Build/Prepare 宏里的命令。
再比如，我们要指定包的安装方法：
``` bash
define Package/helloworld/install
    $(INSTALL_DIR) $(1)/bin
    $(INSTALL_BIN) $(PKG_BUILD_DIR)/helloworld $(1)/bin/
endef
```
上面的这个宏就是指定了包的安装过程。其中 INSTALL_DIR 定义在 rules.mk 文件里。

INSTALL_DIR = install -d -m0755

INSTALL_BIN = install -m0755

$(1)为第一个参数是./configure时的--prefix参数，通常为""

展开之后就是：
``` bash
define Package/helloworld/install
    install -d -m0755 /bin
    install -m0755 $(PKG_BUILD_DIR)/helloworld /bin/
endef
```
其他的可选定义宏就不在这里展开了，可以到官网上去查阅详细内容。
#### 二、使之生效
在Makefile的最后一行是：
``` bash
$(eval $(call BuildPackage,helloworld))
```
最重要的 BuildPackage定义在 package.mk 文件里。


**注意：本人在制作ipk包的过程中遇到两个错误导致配置的包无法在menuconfig界面看到要编译的包：１. 配置的目录结构太深（当时在package目录下五层目录创建的包）；２.两个包目录文件名相同导致只能显示一个**




本文主要参考一下文章：
[OpenWRT开发之——创建软件包(有更新)](https://my.oschina.net/hevakelcj/blog/410633)
[OpenWRT开发之——研究包的Makefile](https://my.oschina.net/hevakelcj/blog/411942)

