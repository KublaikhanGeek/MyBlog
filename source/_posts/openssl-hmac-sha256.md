title: openssl hmac-sha256
date: 2016-02-06 21:21:13
tags: [server开发,openssl]
categories: server开发
---

## 使用openssl生成sha256的数字签名
命令如下：
```bash
 openssl dgst -binary -sha256 -mac HMAC -macopt hexkey:$kSecret $filename
```
参考文档：
1.[openssl hmac differ from python hmac](http://stackoverflow.com/questions/30974080/openssl-hmac-differ-from-python-hmac)
2.[HMAC使用Linux内核加密api不一样,通过OpenSSL命令](http://www.28im.com/c/a2650534.html)
3.[openssl命令行入门](http://fumin.hustbackup.cn/?p=449)
4.[Using openssl to generate HMAC using a binary key](http://nwsmith.blogspot.com/2012/07/using-openssl-to-generate-hmac-using.html)
5.[AWS Signature v4 in Bash](http://danosipov.com/?p=496)
6.[HMAC-SHA1 in bash](http://stackoverflow.com/questions/7285059/hmac-sha1-in-bash)
7.[S3-AWSV4-upload.sh](https://github.com/wachtelbauer/linux-shell-scripts/blob/master/S3-AWS4-Upload.sh)
