# 各种换源配置

## kali

修改`/etc/apt/sources.list`
```
deb https://mirrors.aliyun.com/kali kali-rolling main non-free contrib
deb-src https://mirrors.aliyun.com/kali kali-rolling main non-free contrib
```

## python

添加文件`~/.pip/pip.conf`
```conf
[global]
index-url = https://mirrors.aliyun.com/pypi/simple/

[install]
trusted-host=mirrors.aliyun.com
```

## npm

执行命令
`npm set registry https://registry.npm.taobao.org`

## crates

修改`~/.cargo/config`
```conf
[source.crates-io]
replace-with = 'tuna'

[source.tuna]
registry = "https://mirrors.tuna.tsinghua.edu.cn/git/crates.io-index.git"
```