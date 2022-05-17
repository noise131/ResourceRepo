[TOC]

***

# 安装编译 python 所需的依赖包

```shell
]# yum install -y zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel 
```

# 下载并解压 python 源码包

```shell
]# wget https://www.python.org/ftp/python/3.6.10/Python-3.6.10.tgz
]# tar -xvf Python-3.6.10.tgz
```

# 编译安装 python

```shell
]# cd Python-3.6.10

# 预编译
]# ./configure prefix=/usr/local/python3

# 编译
]# make && make install
```

# 创建软连接

```shell
]# ln -sf /usr/local/python3/bin/python3 /usr/bin/python3
]# ln -sf /usr/local/python3/bin/pip3 /usr/bin/pip3
]# ln -sf /usr/bin/pip3 /usr/bin/pip

# 测试
]# python3 --version
Python 3.6.10
]# pip3 --version
pip 18.1 from /usr/local/python3/lib/python3.6/site-packages/pip (python 3.6)
```

# 修改默认 python 指向

centos7 默认的 python 程序为 python2 如果系统中依赖 python2 的程序过多可以不修改默认的 python 指向

```shell
# 默认的 python 指向
]# ls -l /usr/bin/python
lrwxrwxrwx. 1 root root 7 Jan 28  2021 /usr/bin/python -> python2
```

## 1. 修改默认 python 指向

```shell
]# ln -sf /usr/bin/python3 /usr/bin/python
]# ls -l /usr/bin/python
lrwxrwxrwx 1 root root 16 May  5 14:13 /usr/bin/python -> /usr/bin/python3

# 测试
]# python --version
Python 3.6.10
# 默认的 python 已经指向 python3，而原 python2 则需要使用 python2 调用
]# python2 --version
Python 2.7.5
```

## 2. 修改 yum 的 python 位置

yum 使用 python2 但是将 python 指向 python3 之后 yum 就会使用 python3 导致版本过高报错。

```shell
# 编辑 yum
]# vim /usr/bin/yum
#将第一行 #!/usr/bin/python 修改 python2 ，如下
#!/usr/bin/python2

# 测试 yum 是否可用
]# yum repolist
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
repo id                                              repo name                                                                 status
base/7/x86_64                                        CentOS-7 - Base - mirrors.aliyun.com                                      10,072
docker-ce-stable/7/x86_64                            Docker CE Stable - x86_64                                                    150
epel/x86_64                                          Extra Packages for Enterprise Linux 7 - x86_64                            13,753
extras/7/x86_64                                      CentOS-7 - Extras - mirrors.aliyun.com                                       509
updates/7/x86_64                                     CentOS-7 - Updates - mirrors.aliyun.com                                    3,728
repolist: 28,212
```

修改另外一个文件，主要是下载软件包功能，也需要修改为 python2

```shell
]# vim /usr/libexec/urlgrabber-ext-down
# 修改 python 为 python2
#! /usr/bin/python2
#  A very simple external downloader
#  Copyright 2011-2012 Zdenek Pavlas

#   This library is free software; you can redistribute it and/or
#   modify it under the terms of the GNU Lesser General Public
#   License as published by the Free Software Foundation; either
#   version 2.1 of the License, or (at your option) any later version.
```

# 修改 pip 源

将 pip 源配置为清华源

```shell
# 针对指定用户配置的源
]# pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
Writing to /root/.config/pip/pip.conf

]# cat ~/.config/pip/pip.conf 
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple

# 检查
]# pip config list
global.index-url='https://pypi.tuna.tsinghua.edu.cn/simple'
```































