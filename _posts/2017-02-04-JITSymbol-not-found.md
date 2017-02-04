---
layout: post
title: JITSymbol.h找不到
date: 2017-02-04 22:20:00 +0800
---

用软件源的llvm-dev时，坑爹的缺少很多头文件，自己编译了官网的源码才发现。

然而安装了自己编译的源码依然有找不到的头文件`JITSymbol.h`，经google在llvm的github
镜像中找到了这个文件，可是在官网下载的源码里竟然没有，取而代之的是一个`JITSymbolFlags.h`
的文件。无奈之下，只能用github上的源码重编译安装。

这种问题好奇怪，是我对llvm不够了解还是什么，为什么会有这种问题。
