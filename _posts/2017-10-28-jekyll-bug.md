---
layout: post
title: jekyll神bug
data: 2017-10-28 15:19:00 +0800
---

突然有一天jekyll开始报错：
`Liquid Exception: no implicit conversion of Fixnum into String in /_layouts/default.html`
断断续续找解决方法，试了好多都不行，应该是github更新了jekyll版本的问题，jekyll之前的版本
没有问题，更新后出现了。

今天新建了个jekyll项目，把我原来的博客都导进去，一点一点改，最后定位到了bug点，是`_config.yml`
里面的title，我用的纯数字，会导致无法转换为字符串，通过加上引号完美解决。

简直了，更新了也不说明一下，坑了我好久。。。
