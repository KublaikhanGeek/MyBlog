title: c++ 继承内存布局
date: 2021-08-19 17:11:10
tags:
---
### c++ 继承内存布局

[C++对象内存模型](https://tangocc.github.io/2018/03/20/cpp-class-memory-struct/)
[C++对象模型：单继承，多继承，虚继承，菱形虚继承，及其内存布局图](https://www.shuzhiduo.com/A/Gkz1M6NqzR/)


### 虚基类的初始化
1. 对于虚基类的初始化是由最后的派生类中负责初始化。
2. 在最后的派生类中不仅要对直接基类进行初始化，还要负责对虚基类初始化。