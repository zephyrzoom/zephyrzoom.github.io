---
layout: post
title: "基于hex-rays缓冲区溢出漏洞挖掘"
date: 2016-05-09 15:17:00 +0800
categories: jekyll update
---
漏洞挖掘技术有：手工测试、Fuzzing测试、二进制比对、静态分析、动态分析。

程序结构的表示：使用AST。

动态二进制修正反编译结果：动态执行过程种识别函数入口和调用地址，添加到注释，用来对间接调用修复。

基于库函数参数反向修正：通过所调用库函数的参数，识别出变量的类型，反向通过AST修正前面的错误的变量类型。

```c
void copy_str(char *dest, char *src)
{
    int n;
    if (strlen(src) == 0)
        return;
    n = strlen(src);
    memcpy(dest, src, n);
    return;
}
```
如果src的长度超过dest的长度就会溢出。

```c
void func(char *str)
{
    char buf[32];
    int i = 0;
    if (str == null) return;
    memset(buf, 0, sizeof(buf));
    while (*str != ' ') {
        if (*str == '\r' || *str == '\n')
            break;
        buf[i] = *str;
        ++i;
        ++str;
    }
    ...
    return;
}
```
使用上面的函数，如果没有遇到'\r'过着'\n'则可能会一直复制导致buf溢出。

![stackoverflow](/image/stackoverflow.png)
dest参数是一个数组，由于栈空间是向低地址增长，而缓冲区向高地址增长。所以通过溢出缓冲区可以覆盖函数的返回地址，使返回地址跳转到shellcode。
