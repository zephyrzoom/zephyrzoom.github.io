---
layout: post
title: lldb无法调试string类型
date: 2017-02-04 22:40:00 +0800
---

用clang++编译使用了string类型的代码时，再用lldb调试时，无法查看string变量的值。

例如这段代码：

```c++
// test.cpp
#include <string>

int main(int argc, char const argv[]) {
    std::string a = "hello";
    std::string b = "world";
    return 0;
}
```

编译这段代码：`clang++ test.cpp -g`

调试：`lldb a.out`

在第4行加入断点，运行到定义`a`时，查看其值`p a`会报错：

```
error: incomplete type 'string' (aka 'std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> >') where a complete type is requ
ired

note: forward declaration of 'std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> >'
error: 1 errors parsing expression
```

**解决办法**：安装libstdc++6-5-dbg
