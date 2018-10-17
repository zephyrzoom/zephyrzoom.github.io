---
layout: post
title: nginx配置
date: 2018-10-13 16:35:00 +0800
---

静态文件访问的配置
```
location /publish {
    root /home/mcdp/packages;
}
```
访问`localhost:80/publish`就会从`/home/mcdp/packages/publish`目录下获取文件