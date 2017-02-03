---
layout: post
title: llvm::errs()找不到
date: 2017-02-03 16:33:00 +0800
---

1. 查询[doxygen](http://llvm.org/doxygen/)
2. 点击Namespaces菜单
3. 找到llvm命名空间
4. ctrl-f查找errs
5. 点进[errs](http://llvm.org/doxygen/namespacellvm.html#ab8e34eca3b0817ef7a127913fbf6d9e4)看到definition at `raw_ostream.cpp`
6. 在[raw_ostream.cpp](http://llvm.org/doxygen/raw__ostream_8cpp_source.html)代码中可以看到`#include`的同名头文件
7. `llvm/Support/raw_ostream.h`就是errs所在的头文件
