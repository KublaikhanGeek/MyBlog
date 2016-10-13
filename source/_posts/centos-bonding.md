title: centos bonding
date: 2016-02-16 16:21:16
tags: [运维,bonding]
categories: 运维
---

网卡绑定技术在不同的平台下的叫法不同，在Linux下叫bonding，IBM称为etherchanel，broadcom叫team，但效果都是将两块或更多的网卡当做一块网卡使用，在增加带宽的同时也可以提高冗余性。一般使用较多的就是来提高冗余，分别和不同交换机相连，提高可靠性，但有时服务器带宽不够了也可以增加带宽。
双网卡绑定就是使用两块网卡虚拟成为一块网卡，这个聚合起来的设备看起来是一个单独的以太网接口设备，通俗点讲就是两块网卡具有相同的IP地址而并行链接聚合成一个逻辑链路工作。其实这项技术在Sun和Cisco中早已存在，被称为Trunking和Etherchannel技术，在 Linux的2.4.x的内核中也采用这这种技术，被称为bonding。bonding技术的最早应用是在集群——beowulf上，为了提高集群节点间的数据传输而设计的。下面我们讨论一下bonding 的原理，什么是bonding需要从网卡的混杂(promisc)模式说起。在正常情况下，网卡只接收目的硬件地址(MAC Address)是自身Mac的以太网帧，对于别的数据帧都滤掉，以减轻驱动程序的负担。但是网卡也支持另外一种被称为混杂promisc的模式，可以接 收网络上所有的帧，比如说tcpdump，就是运行在这个模式下。bonding也运行在这个模式下，而且修改了驱动程序中的mac地址，将两块网卡的Mac地址改成相同，可以接收特定mac的数据帧。然后把相应的数据帧传送给bond驱动程序处理。
<!--more-->
## 一、服务器双网卡绑定
查看Linux是否支持bonding，RHEL4已经默认支持了。
``` bash
# modinfo bonding
```

### 1、创建/etc/sysconfig/network-scripts/ifcfg-bond0
``` bash
DEVICE=bond0
BOOTPROTO=static
IPADDR=192.168.0.254
BROADCAST=192.168.0.255
NETMASK=255.255.255.0
NETWORK=192.168.0.0
GATEWAY=192.168.0.1
USERCTL=no
ONBOOT=yes
TYPE=Ethernet
#注意：建议不要指定MAC地址
```

### 2、修改/etc/sysconfig/network-scripts/ifcfg-eth0
``` bash
DEVICE=eth0
BOOTPROTO=none
ONBOOT=yes
USERCTL=no
MASTER=bond0
SLAVE=yes
#注意：建议不要指定MAC地址
```

### 3、修改/etc/sysconfig/network-scripts/ifcfg-eth1
``` bash
DEVICE=eth1
BOOTPROTO=none
ONBOOT=yes
USERCTL=no
MASTER=bond0
SLAVE=yes
#注意：建议不要指定MAC地址
```

### 4、修改/etc/modprobe.d/dist.conf
``` bash
alias bond0 bonding
options bond0 miimon=100 mode=0
#（mode=0，表示平衡负载双网卡工作，RR算法，mode=1，自动主备，其中一块工作）
#（miimon=100，miimon是指多久时间要检查网路一次，单位是ms(毫秒) ,意思是假设其中有一条网路断线，会在0.1秒内自动备援）
```

### 5、配置完成，重启OS，就可以看到有一张bond0的新网卡。
### 6、查看bond0当前状态：
``` bash
# cat /proc/net/bonding/bond0
```

## 二、思科交换机相应端口配置端口聚合
``` bash
R1#conf t
R1(config)#int range g0/1 - 2 
R1(config-int-range)#channel-group 1 mode on
``` 
将交换机g0/1-2与服务器相应端口相连，经测试带宽明显增加。
如果不接服务器两端口分别接不同交换机则不需要做交换机端口聚合。
