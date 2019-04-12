title: ssh框架学习
date: 2017-01-01 13:48:36
tags: [java, javaEE, ssh, 框架]
categories: javaEE
---
本文主要总结在学习javaEE过程中的知识点以及遇到的坑
其实在搭建SSH框架过程中遇到最多的问题就是jar包和配置的问题，目前我是基于spring-.3.5、strust2-2.5.8、hibernate-5.0.1搭建的SSH框架
<!--more-->
##### 一、jar包的管理
目前主要是手动添加的jar包，SSH框架的jar包实在是太多，看着都头痛，还没有理解每个jar包的作用。以下是目前整理的jar包
``` bash
------hibernate-5.2.6-------
antlr-2.7.7.jar
cdi-api-1.1.jar
classmate-1.3.0.jar
dom4j-1.6.1.jar
aspectjweaver.jar
el-api-2.2.jar
geronimo-jta_1.1_spec-1.1.1.jar
hibernate-commons-annotations-5.0.1.Final.jar
hibernate-core-5.2.6.Final.jar
hibernate-jpa-2.1-api-1.0.0.Final.jar
jandex-2.0.3.Final.jar
javassist-3.20.0-GA.jar
javax.inject-1.jar
jboss-interceptors-api_1.1_spec-1.0.0.Beta1.jar
jboss-logging-3.3.0.Final.jar
jsr250-api-1.0.jar
slf4j-api-1.7.7.jar
slf4j-log4j12-1.7.7.jar
mysql-connector-java-5.1.28.jar

-------struts2-2.5.8---------
commons-fileupload-1.3.2.jar
commons-io-2.4.jar
commons-lang3-3.4.jar
commons-logging-1.1.3.jar
freemarker-2.3.23.jar
log4j-1.2.17.jar
log4j-api-2.7.jar
ognl-3.1.12.jar
struts2-core-2.5.8.jar
struts2-spring-plugin-2.5.8.jar
javassist-3.20.0-GA.jar


-------spring-4.3.5------
spring-aop-4.3.5.RELEASE.jar
spring-aspects-4.3.5.RELEASE.jar
spring-beans-4.3.5.RELEASE.jar
spring-context-4.3.5.RELEASE.jar
spring-context-support-4.3.5.RELEASE.jar
spring-core-4.3.5.RELEASE.jar
spring-expression-4.3.5.RELEASE.jar
spring-instrument-4.3.5.RELEASE.jar
spring-instrument-tomcat-4.3.5.RELEASE.jar
spring-jdbc-4.3.5.RELEASE.jar
spring-jms-4.3.5.RELEASE.jar
spring-messaging-4.3.5.RELEASE.jar
spring-orm-4.3.5.RELEASE.jar
spring-oxm-4.3.5.RELEASE.jar
spring-test-4.3.5.RELEASE.jar
spring-tx-4.3.5.RELEASE.jar
spring-web-4.3.5.RELEASE.jar
spring-webmvc-4.3.5.RELEASE.jar
spring-webmvc-portlet-4.3.5.RELEASE.jar
spring-websocket-4.3.5.RELEASE.jar
commons-dbcp2-2.1.1.jar
commons-pool2-2.4.2.jar
```
##### 二、Java EE坑

> 
* **错误：**there is no Action mapped for namespace [/] and action name [user!list] associated with context path [/ssh_user]
	网上好多人遇到了同样的错误，大部分人的问题在于：`１．struts.xml文件放错位置。２．struts.xml文件名错误`
    我确认不存在这些问题，google后发现`WebAppLibraries>>struts2-core-2.5.8.jar>>org.apache.struts2>>default.properties找到struts.enable.DynamicMethodInvocation属性，这个默认是false，需要打开这个配置`
* **解决：**我们在struts.xml文件中添加`<constant name="struts.enable.DynamicMethodInvocation" value="true"></constant>`就打开了动态调用的配置项

***
> 
* **错误：**java.sql.SQLException: Access denied for user ''@'localhost' (using password: YES)
* **分析：**由于该项目不是自己创建的是从github上clone后导入我的eclipse的，下载下来后看到文件的编码格式为：ISO-8859-1,而我自己的默认编码格式为UTF-8，因此怀疑是文件格式导致的，不管怎么说先解决编码格式的问题再说
* **解决：**没有找到编码格式的转换方法，就逐个文件编辑改成了UTF-8的编码格式，问题就好了，所以以后最好都同一成UTF-8的编码格式

***