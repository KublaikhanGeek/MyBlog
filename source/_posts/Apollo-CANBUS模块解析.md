title: Apollo CANBUS模块解析
date: 2019-04-19 16:12:47
tags: [Apollo,CAN,自动驾驶,人工智能]
categories: AI
---
# CANBUS模块解析
<!-- toc -->

[TOC]
### 一、CANBUS目录结构
![Alt text](./1555597041738.png)
<!--more-->

### 二、如何根据车厂DBC文件生成车厂代码
工具位于apollo/modules/tools/gen_vehicle_protocol目录，使用以下命令生成车厂代码：
``` bash
python gen.py lincoln_conf.yml
```
会在当前目录下生成output目录，目录里包含proto和vehicle两个目录，proto目录里的xxx.proto文件是汽车的具体报文信息，vehicle目录生成以下文件：
![Alt text](./1555645006674.png)
所以该工具根据DBC文件帮你完成了大量的编码工作。

### 三、相关文件描述
* apollo/modules/canbus/chassis_detail.proto：该文件的作用是保存车辆自定义参数。使用DBC文件生成自己的proto文件，拷贝到chassis_detail.proto的下面。不用担心自己生成的proto文件能否被Apollo识别，因为Apollo要处理的参数并不是chassis_detail.proto，而是apollo/modules/canbus/proto/chassis.proto。你需要在自己的controller.cc中将chassis_detail.proto转换为chassis.proto。
* apollo/modules/canbus/chassis.proto：Apollo中的一个message。主要用来保存地盘的通用参数。这些参数通过转换车辆自定义的chassis_detail.proto得到。转换函数在自己的conroller.cc中实现
* 在上一步中生成了车辆lincoln文件夹，在这个文件夹中，lincoln_controller.cc就是用来将其他模块发出来的message转换成底盘可以执行的数据，同时也将底盘的返回的数据转换成Apollo需要的message返回给其他模块。lincoln_message_manager.cc管理CAN的protocol data。lincoln_vehicle_factory.cc将lincoln添加进入Apollo。
###四、CANBUS模块类图
![Alt text](./1555646066648.png)

### 五、数据流
CANBUS数据流图
![Alt text](./1555648906673.png)
<font color=red>注：<font color=yellow>黄色箭头</font>是读取车身信息发布话题消息的数据流，<font color=blue>蓝色箭头</font>是订阅话题消息发布CAN指令的数据流</font>

### 六、函数调用
##### 1. 读取车身信息并发布话题消息的函数调用
![Alt text](./1555659062409.png)

##### 2. 订阅话题消息并发送CAN指令的函数调用
![Alt text](./1555659844827.png)

### 参考文献
* [Apollo2.5 CANBUS调试笔记(测试版)](https://www.cnblogs.com/hgl0417/p/10242684.html)


