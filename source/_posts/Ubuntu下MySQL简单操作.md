title: Ubuntu下MySQL简单操作
date: 2016-11-10 18:20:03
tags: [数据库]
categories: javaee
---

原文链接：[http://www.jianshu.com/p/694d7d0a170b](http://www.jianshu.com/p/694d7d0a170b)

### 安装
Ubuntu下安装常规软件都比较简单，直接使用apt-get即可。安装步骤如下：

``` bash
$ sudo apt-get install mysql-server mysql-client
```
安装过程中会提示设置root密码，注意设置了不要忘了，安装完成之后可以使用如下命令来检查是否安装成功：

``` bash
$ sudo netstat -tap | grep mysql
```
通过上述命令检查之后，如果看到有mysql 的socket处于 listen 状态则表示安装成功。

<!--more-->

### 登陆mysql数据库

可以通过如下命令：
``` bash
$ mysql -u root -p
$ mysql -h localhost -u root -p
```
>-u 表示选择登陆的用户名
-p 表示登陆的用户密码
-h 登录主机名

### 服务启动、停止
#### 1. 启动方式

* 1、使用 service 启动：
``` bash
service mysql start
```

* 2、使用 mysqld 脚本启动
``` bash
/etc/inint.d/mysql start
```

* 3、使用 safe_mysqld 启动
``` bash
safe_mysql&
```
查看mysql是否在监听端口命令
``` bash
$netstat -tl | grep mysql
```
会看到如下类似内容
``` bash
tcp 0 0 *:mysql *:* LISTEN
```
#### 2. 停止


* 1、使用 service 启动

``` bash
service mysql stop
```

* 2、使用 mysqld 脚本启动

``` bash
/etc/inint.d/mysql stop
```
#### 3. 重启


* 1、使用 service 启动

``` bash
service mysql restart
```

* 2、使用 mysqld 脚本启动

``` bash
/etc/inint.d/mysql restart
```

* 3、mysqladmin shutdown

### 数据类型

MySQL有三大类数据类型, 分别为数字、日期\时间、字符串, 这三大类中又更细致的划分了许多子类型:
#### 1. 数字类型

* 整数:

tinyint、smallint、mediumint、int、bigint

* 浮点数:

float、double、real、decimal

* 日期和时间:

date、time、datetime、timestamp、year

#### 2. 字符串类型

* 字符串:

char、varchar

* 文本:

tinytext、text、mediumtext、longtext

* 二进制(可用来存储图片、音乐等):

tinyblob、blob、mediumblob、longblob

### 常用命令
``` bash
create database new_dbname;--新建数据库
drop database old_dbnane; --删除数据库
show databases;--显示数据库
use databasename;--使用数据库
select database();--查看已选择的数据库

show tables;--显示当前库的所有表
create table tablename(fieldname1 fieldtype1,fieldname2 fieldtype2,..)[ENGINE=engine_name];--创建表
drop table tablename; --删除表
create table tablename select statement;--通过子查询创建表
desc tablename;--查看表结构
show create table tablename;--查看建表语句

alter table tablename add new_fielname new_fieldtype;--新增列
alter table tablename add new_fielname new_fieldtype after 列名1;--在列名1后新增列
alter table tablename modify fieldname new_fieldtype;--修改列
alter table tablename drop fieldname;--删除列
alter table tablename_old rename tablename_new;--表重命名

insert into tablename(fieldname1,fieldname2,fieldnamen) valuse(value1,value2,valuen);--增
delete from tablename [where fieldname=value];--删
update tablename set fieldname1=new_value where filename2=value;--改
select * from tablename [where filename=value];--查

truncate table tablename;--清空表中所有数据，DDL语句

show engines;--查看mysql现在已提供的存储引擎:
show variables like '%storage_engine%';--查看mysql当前默认的存储引擎
show create table tablename;--查看某张表用的存储引擎（结果的"ENGINE="部分）
alter table tablename ENGINE=InnoDB--修改引擎
create table tablename(fieldname1 fieldtype1,fieldname2 fieldtype2,..) ENGINE=engine_name;--创建表时设置存储引擎
```
通过脚本文件操作：
``` bash
mysql -D samp_db -u root -p < createtable.sql
```



