---
title: 第四章 windows实战-emlog
subtitle:
date: 2025-03-14T14:56:09+08:00
slug: 4583f0a
draft: false
author:
  name: T1ng
  link:
  email:
  avatar: /images/avater.jpg
description:
keywords:
license:
comment: false
weight: 0
tags:
  - 玄机
categories:
  - 玄机
hiddenFromHomePage: false
hiddenFromSearch: false
hiddenFromRelated: false
hiddenFromFeed: false
summary:
resources:
  - name: featured-image
    src: featured-image.jpg
  - name: featured-image-preview
    src: featured-image-preview.jpg
toc: true
math: false
lightgallery: false
password:
message:
repost:
  enable: true
  url:

# See details front matter: https://fixit.lruihao.cn/documentation/content-management/introduction/#front-matter
---

<!--more-->

<!-- Place resource files in the current article directory and reference them using relative paths, like this: `![alt](images/screenshot.jpg)`. -->

```
通过本地 PC RDP到服务器并且找到黑客植入 shell,将黑客植入 shell 的密码 作为 FLAG 提交;
通过本地 PC RDP到服务器并且分析黑客攻击成功的 IP 为多少,将黑客 IP 作为 FLAG 提交;
通过本地 PC RDP到服务器并且分析黑客的隐藏账户名称,将黑客隐藏账户名称作为 FLAG 提交;
通过本地 PC RDP到服务器并且分析黑客的挖矿程序的矿池域名,将黑客挖矿程序的矿池域名称作为(仅域名)FLAG 提交;
```

远程连接，发现桌面有个`phpstudy`找到其中的`www`的目录。
![](images/0faeddd22efc2287dc51d6563ed51054.png)
![](images/b483742e029fc692ab2000ca8fd5cc74.png)
用D盾扫一下，查找到`shell.php`木马文件的位置
![](images/1980b7936f2e8d3ede37cf9a8239faf2.png)
查看一下`shell.php`文件，默认密码`rebeyond`，
![](images/f776b5e9edf6b86b671aacad72ca5cbe.png)
第一问：flag{rebeyond}

去查找日志文件，发现Nginx的目录的大小为0，所以存在在Apache日志文件，打开日志文件，ctrl+f查找一下`shell.php`
![](images/405da42893e994ecdfa4f383b3677519.png)
发现ip为`192.168.126.1`多次利用`shell.php`进行代码执行或文件操作。
第二问：flag{192.168.126.1}

找出黑客隐藏的用户名称，检查靶机的用户组即可
![](images/2459966c0ab5426f10c929d1322241c0.png)
发现可疑用户
![](images/e41b749eac5be5257a8b2a59d1af3ce0.png)
第三问：flag{hacker138}

查找一下hacker用户下的文件
![](images/28c6aba39891c59212bfbe1c630c3949.png)
反编译一下
[pyinstxtractor/pyinstxtractor.py at master · extremecoders-re/pyinstxtractor](https://github.com/extremecoders-re/pyinstxtractor/blob/master/pyinstxtractor.py)
![](images/71d010f382f4d0cb8d12857b8504e899.png)
![](images/1c1588f235c153ca4d9545ca1ccb06df.png)
010查看一下文件，找到了域名
![](images/4fd4d166ef6a9294c00dabe0e4b002bd.png)
第四问：flag{wakuang.zhigongshanfang.top}
