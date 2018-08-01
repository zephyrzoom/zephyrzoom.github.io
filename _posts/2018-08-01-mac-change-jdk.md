---
layout: post
title: Mac切换JDK
date: 2018-08-01 09:50:00 +0800
---

运行`/usr/libexec/java_home -V`查看已有JDK版本

```
Matching Java Virtual Machines (2):
    9.0.1, x86_64:      "Java SE 9.0.1" /Library/Java/JavaVirtualMachines/jdk-9.0.1.jdk/Contents/Home
    1.8.0_171, x86_64:  "Java SE 8"     /Library/Java/JavaVirtualMachines/jdk1.8.0_171.jdk/Contents/Home

/Library/Java/JavaVirtualMachines/jdk-9.0.1.jdk/Contents/Home
```

选择一个版本设置到zshrc：
```bash
export JAVA_HOME=`/usr/libexec/java_home -v 1.8.0_171`
```