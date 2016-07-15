---
layout: post
title: virtualenv安装
date: 2016-07-15 12:35:00 +0800
---
通过`sudo pip install virtualenv`安装，记得加sudo，不加安装完命令找不到不知为何。

使用时，先在工程中新建一个文件夹`venv`，然后运行`virtualenv venv`即可创建一个虚拟运行环境。

使虚拟环境生效，需要`cd venv/bin`，然后运行`source activate`环境生效。
