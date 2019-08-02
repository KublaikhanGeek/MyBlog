title: cjscon library
date: 2019-08-01 19:44:44
tags: [library]
categories: library
---

### cJSON库使用的注意事项
****
1. cJSON 按照json的数据格式使用了链表的数据结构所以在调用**cJSON_CreateObject()**和**cJSON_Parse()**接口后都需要调用**cJSON_Delete()**接口释放内存, 释放内存会从头节点遍历链表释放所有节点内存。
2. 不能将同一个节点放入两个链表中，这样在释放内存时会重复释放导致崩溃，例如：将解析的某个节点直接挂到新创建的链表中，就会出现这种情况。
3. 在调用**cJSON_PrintUnformatted()**和**cJSON_Print()**接口后要手动释放堆上申请的内存。