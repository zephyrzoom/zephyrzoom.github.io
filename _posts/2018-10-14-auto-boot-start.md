---
layout: post
title: 开机自启动配置
date: 2018-10-14 18:42:00 +0800
---

Linux配置开机自启动程序时，需要创建文件`/etc/rc.local`

在该文件中加入需要执行的命令

注意需要给该文件添加可执行权限：`chmod +x /etc/rc.local`