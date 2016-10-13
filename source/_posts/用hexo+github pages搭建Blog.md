layout: build
title: 用hexo+github pages搭建Blog 
date: 2015-11-13 18:26:26
tags: [运维,网站]
categories: 运维
---
### 用hexo+github pages搭建Blog 
***
>本文主要介绍如何在Windows平台上利用hexo搭建静态blog网站，并使用github进行托管，改天在ubuntu下搭建一下。

<!--more-->

>
花了两天的时间，折腾搭建自己的**Blog**，首先就花了好长时间来选择使用什么工具搭建，发现现在好多人使用github来托管自己的静态网页博客，另外自己也在探索github中，也选择使用github来托管自己的网站。接下来需要选择一个博客框架，主要比较了jekll、Octopress和hexo这三个目前使用较多的，最终选择了hexo，主要基于以下几点：
* 轻巧快速，相较于其他两个，更加轻量级，而且速度更快
* 配置使用更加简单
* hexo也可以兼容Octopress很多插件
* hexo对中文的支持更加好 

好了接下来就开始搭建我们自己的博客吧！
#### 搭建本地hexo博客
##### 1. 安装Node.js
到nodejs官网（[https://nodejs.org/en/](https://nodejs.org/en/)）下载nodejs程序安装即可
##### 2. 安装hexo
在Git bash命令行下执行以下命令：

	npm install -g hexo
	
##### 3. 初始化
创建hexo静态网站目录（任意位置，如F:\myblog)，进入到该目录下鼠标右键git bash执行以下命令，hexo就会在目标文件夹生成建立blog所需要的所有文件

	hexo init
##### 4.安装依赖包
	npm install
##### 5.查看本地搭建的博客
经过以上四步我们就在本地搭建好了本地的博客，再执行以下两个命令我们就可以在浏览器中输入localhost:4000查看我们的博客了

	hexo generate #生成静态页面至public目录
	hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
经过以上五个步骤，以及简单的几个命令我们就在本地搭建好了一个博客了，接下在我们将我们本地的blog托管到github，就可以畅快的开始享受写博客的快乐了
#### 本地博客托管到github
首先你先要有一个github的账号，这里就不再赘述了。
##### 1. 创建GitHub Pages仓库
首先简单的介绍一下GitHub Pages，GitHub Pages 分为两种，一种是用你的GitHub用户名建立的username.github.io这样的用户&组织站点，另一种是依附项目的Pages。两种都差不多，第二种还要复杂一点。

想建立个人博客用第一种就可以了，形如username.github.io这样的可访问的站点，每个用户名下面只能建立一个。

点击**new repository**按钮创建新的仓库，Repository name填写Rusername.github.io样式的名字，比如我的就是KublaikhanGeek.github.io

点击**Create repository**按钮，创建仓库完成
##### 2.本地博客上传github远程仓库
修改站点配置文件_config.yml

	deploy:
	  type: git
	  repository: https://github.com/KublaikhanGeek/KublaikhanGeek.github.io
	  branch: master
	
执行下列命令上传

	hexo generate #生成静态页面至public目录
	hexo deploy #将.deploy目录部署到GitHub
会出现ERROR Deployer not found:git错误，需要执行以下命令

	npm install hexo-deployer-git --save
在执行一遍上面的两个命令就可以了，这时候你就看到你本地的博客上传到仓库里了，你可以通过username.github.io访问你的博客了。

***
#### 发表文章
辛辛苦苦折腾了这么久，享受写作的时候终于到了。开心死了呢~~~
在博客目录下进入git bash，输入以下命令：

	hexo new "文章题目"
就会在source\_posts文件夹下生成一个“文章题目.md”的文件，编辑该文件后执行

	hexo generate #生成静态页面至public目录
	hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
就可以上传到仓库，文章就发表了，是不是很简单

注：代码高亮用的是highlight.js。下面的连接可以查看highlight.js支持的语言：

[http://highlightjs.org/static/test.html
](http://highlightjs.org/static/test.html)
***
#### 更换主题
如果你感觉hexo默认的主题不够漂亮，你可以到hexo官网上挑选你喜欢的主题，然后git clone到themes目录下，再然后修改站点配置文件_config.yml的theme属性就可以了，在push到github就发布了
![](http://i12.tietuku.com/8c6dddaea6d6bd35.png)



