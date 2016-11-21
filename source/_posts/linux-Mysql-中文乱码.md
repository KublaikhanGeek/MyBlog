title: linux Mysql 中文乱码
date: 2016-11-21 15:05:51
tags: [数据库]
categories: javaee
---

原文链接：[http://coolnull.com/2424.html](http://coolnull.com/2424.html)

##### 说明：

mysql默认的字符编码是latin1,而我用的是utf-8,存入数据库就变成了?????这样的乱码。windows下只要把my.ini中两处default-character-set=latin1都改为default-character-set=utf8重启既可。

linux下则需要修改/etc/my.cnf，在[mysqld]下加入default-character-set = utf8，[client]下加入default-character-set = utf8，在[mysql]字段里加入default-character-set=utf8

<!--more-->

##### 实现：
1. 查看原编码
``` bash
	mysql> show variables like 'character%'; //执行编码显示，可以看到默认是latin1
	+--------------------------+----------------------------+
	| Variable_name | Value |
	+--------------------------+----------------------------+
	| character_set_client | latin1 |
	| character_set_connection | latin1 |
	| character_set_database | latin1 |
	| character_set_filesystem | binary |
	| character_set_results | latin1 |
	| character_set_server | latin1 |
	| character_set_system | utf8 |
	| character_sets_dir | /usr/share/mysql/charsets/ |
	+--------------------------+----------------------------+
```

2. 修改/etc/my.cnf，分别在[client],[mysqld],[mysql]字段里添加default-character-set=utf8。注意[mysqld]字段与[mysql]字段是有区别的。这点在网上没人反馈过。
``` bash
	#vim /etc/my.cnf  //添加default-character-set=utf8
	[client]
	port = 3306
	socket = /var/lib/mysql/mysql.sock
	default-character-set=utf8

	[mysqld]
	port = 3306
	socket = /var/lib/mysql/mysql.sock
	character-set-server=utf8

	[mysql]
	no-auto-rehash
	default-character-set=utf8
```
修改完成后，service mysql restart重启mysql服务器，使用SHOW VARIABLES LIKE ‘character%’;查看，发现数据库编码全已改成utf8
```bash
	mysql> show variables like 'character%';
	+--------------------------+----------------------------------------+
	| Variable_name            | Value                                  |
	+--------------------------+----------------------------------------+
	| character_set_client     | utf8                                   |
	| character_set_connection | utf8                                   |
	| character_set_database   | utf8                                   |
	| character_set_filesystem | binary                                 |
	| character_set_results    | utf8                                   |
	| character_set_server     | utf8                                   |
	| character_set_system     | utf8                                   |
	| character_sets_dir       | /usr/local/mysql/share/mysql/charsets/ |
	+--------------------------+----------------------------------------
```

3. 如果上面的都修改了还乱码，那剩下问题就一定在connection连接层上。解决方法是在发送查询前执行一下下面这句（直接写在SQL文件的最前面）：
SET NAMES ‘utf8′;它相当于以下三指令：
SET character_set_client = utf8;
SET character_set_results = utf8;
SET character_set_connection = utf8;

##### 附录：
朋友的一个站点转到我这边。导入mysql时，mysql还是latin的编码，因此，虽然按上面的步骤，在/etc/my.cnf文件中[client],[mysqld],[mysql]字段里添加default-character-set=utf8，但站点部分中文是乱码。在将mysql编辑改为utf8再重新导入数据库后，就正常了。
估计是mysql是latin编码是，导入数据时，因为mysql无法识别，直接将数据保存成了???这种乱码的形式了
