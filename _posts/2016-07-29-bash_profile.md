---
layout: post
title: 新建bash_profile
date: 2016-07-29 12:10:00 +0800
---
新建空的bash_profile之后，终端配色失效，需要先加载bashrc

```shell
if [ -f ~/.bashrc ]; then
  . ~/.bashrc
fi
```
