---
layout: post
title: 证书
date: 2018-10-25 16:46:00 +0800
---

将p12证书转换为pem格式:`openssl pkcs12 -in file.p12 -out file.pem`

关于p12证书的相关操作:[openssl pkcs12操作](https://www.openssl.org/docs/man1.1.0/apps/pkcs12.html)

p12证书其实是将公钥、私钥和可能包括的证书链压缩在一个文件

生成证书:`openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365`

生成p12证书:`openssl pkcs12 -export -out server.p12 -inkey server.key -in server.crt -certfile CACert.crt`

p12生成私钥:`openssl pkcs12 -in server.p12 -nodes -nocerts -out server.key`

p12生成公钥:`openssl pkcs12 -in server.p12 -nokeys -clcerts -out server.crt`

生成带扩展信息的证书
1. 先在系统中搜索的openssl配置文件`find / -name openssl.cnf`
2. 将找到的配置文件cp一份
3. 修改其中配置
在文件最后添加内容
```
[ alternate_names ]

DNS.1 = example.com
DNS.2 = www.example.com
IP.1 = 1.1.1.1
IP.2 = 2.2.2.2
```
在`[ v3_ca ]`区域中添加
```
subjectAltName = @alternate_names
keyUsage = digitalSignature, keyEncipherment
```
解注`[ CA_default ]`中的`copy_extensions = copy`
解注`req_extensions = v3_req`
4. 执行生成证书命令`openssl req -x509 -newkey rsa:2048 -keyout key.pem -out cert.pem -days 5000 -config openssl.cnf`
5. 将证书合并为p12证书`openssl pkcs12 -export -out server.p12 -inkey key.pem -in cert.pem`
6. 如果没有域名的话，common name、DNS和IP都使用相同的IP