---
layout: post
title: 证书
date: 2018-10-25 16:46:00 +0800
---

将p12证书转换为pem格式:`openssl pkcs12 -in file.p12 -out file.pem`

关于p12证书的相关操作:[openssl pkcs12操作](https://www.openssl.org/docs/man1.1.0/apps/pkcs12.html)

p12证书其实是将公钥、私钥和可能包括的CA证书压缩在一个文件

生成证书:`openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365`

生成p12证书:`openssl pkcs12 -export -out server.p12 -inkey server.key -in server.crt -certfile CACert.crt`