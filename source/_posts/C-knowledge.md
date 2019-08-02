title: C++ knowledge
date: 2019-08-01 20:08:37
tags: [c++]
categories: programming language
---
### c++11 知识点
1. 多线程  
   
   1.1 在创建多线程后，需要调用join()或者detach(),来处理多线程，其中join()不能在创建的现场中调用不然程序会报deadlock的错误。  
   c++对join()函数的说明如下：  
   **错误条件**
    * 若 this->get_id() == std::this_thread::get_id() （检测到死锁）则为 resource_deadlock_would_occur
    * 若线程非法则为 no_such_process
    * 若 joinable 为 false 则为 invalid_argument   

2. 容器
   
    2.1 vector容器   
      - 2.1.1 vector  
        - vector不是线程安全的
        - erase和pop_back调用会导致迭代器失效，可能会导致崩溃
      


