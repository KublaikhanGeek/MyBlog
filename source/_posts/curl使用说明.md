title: curl使用说明
date: 2016-02-06 20:55:18
tags: [server开发,curl]
categories: server开发
---

## 一、curl上传文件([原文](http://blog.charlee.li/upload-with-curl/))
***
模拟`multipart/form-data`形式的form上传文件，命令如下。
``` bash
curl -F "action=upload" -F "filename=@file.tar.gz" http://localhost/action.php
```
<!--more-->

如果使用了-F参数，curl就会以`multipart/form-data`的方式发送POST请求。-F参数以name=value的方式来指定参数内容，如果值是一个文件，则需要以name=@file的方式来指定。

如果通过代理，上面的命令有可能会被代理拒绝，这时需要指定上传文件的MIME类型。
```bash
curl -x myproxy.com:1080 -F "action=upload" -F "filename=@file.tar.gz;type=application/octet-stream" http://localhost/action.php
```

另外，如果不上传文件，则可以使用 -d 参数，这时curl会以`application/x-www-url-encoded`方式发送 POST 请求。
```bash
curl -d "action=del" -d "id=12" http://localhost/action.php
```
## 二、curl post数据脚本([原文](http://blog.cocoabit.com/2013-11-06-mac-xia-shi-yong-curl-post-shu-ju/))

最近因为要测试服务器接口，需要批量向服务器上传图片，写了这个脚本：

```bash
#! /bin/bash

files='ls *.JPG'

for file in $files
do 
    filename="file_key=@$file"

    curl -F "key1=value1" \
    -F "key1=value2" \
    -F $filename \        
    http://path/to/server

    echo "" # 换行
done
```

curl POST 二进制数据 使用 -F 选项，上传文件参数为:

```bash
-F "file_key=@path_to_file"    
```

格式是固定的 @符号后面跟文件路径。
这种方式上传文件使用的 content type 为`multipart/form-data`

还有另外一种形式 post 文字的`application/x-www-form-urlencoded`
```bash
curl -d "key1=text1" -d "key2=text2" http://path/to/server
```
