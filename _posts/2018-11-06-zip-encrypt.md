---
layout: post
title: zip压缩加密
date: 2018-11-06 15:30:00 +0800
---

zip压缩时加密文件：`zip -re backup.zip backup/`，执行时需要输入两次密码，`-r`递归执行，`-e`加密

解密：`unzip -d backup backup.zip`，`-d`解压目录

对比压缩前和解压后的文件大小：`du -h --max-depth=1`