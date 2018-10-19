---
layout: post
title: Django invalid http_host错误
date: 2018-10-19 15:50:00 +0800
---

当在nginx中配置了upstream负载均衡器时，配置的名字为带下划线时，Django会报错。

配置如下：
```
upstream load_balancer {
    server xx.xx.xxx.xxx:xxxx;
    server xx.xx.xxx.xxx:xxxx;
}
```

和Django或者uwsgi的配置都没有关系，只要把load_balancer命名中去掉下划线即可。