---
layout: post
title: clang cannot find -lz
date: 2017-02-03 16:33:00 +0800
---

当使用`clang++ -lz`命令时出现下面错误：
![clang-cannot-find-lz](/image/clang-cannot-find-lz.png)

-lz命令参数是指链接libz.so库，找不到该库就会报错，通过安装`zlib1g-dev`解决。
