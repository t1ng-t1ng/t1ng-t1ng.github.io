---
title: 第六章 流量特征分析-waf 上的截获的黑客攻击流量
subtitle:
date: 2025-03-14T14:45:22+08:00
slug: 8cf1620
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
  enable: false
  url:

# See details front matter: https://fixit.lruihao.cn/documentation/content-management/introduction/#front-matter
---

<!--more-->

<!-- Place resource files in the current article directory and reference them using relative paths, like this: `![alt](images/screenshot.jpg)`. -->

```
1.黑客成功登录系统的密码 flag{xxxxxxxxxxxxxxx}
2.黑客发现的关键字符串 flag{xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx}
3.黑客找到的数据库密码 flag{xxxxxxxxxxxxxxxx}
```

成功登录，通常会发送带有认证信息的http请求，用`POST`筛选一下，筛选掉账号信息不正确的，并按照登陆的url过滤

```
!(http contains "您输入的账号信息不正确") && http.request.method == "POST" && http.request.uri contains "/admin/login.php?rec=login"
```



![](images/f2ee7c03ad0e7debe835653565f7ca1d.png)

第一问：flag{admin!@#pass123}

查找一下含有关键字`flag`的包。并且返回值为`200`

```
http contains "flag"
```

翻了一遍，还是按照时间排序，从大的开始找，192.168.94.59和192.168.32.189的请求报文和回显报文

![](images/f2ee7c03ad0e7debe835653565f7ca1d.png)

第二问：flag{87b7cb79481f317bde90c116cf36084b}

顺着第二问，过滤http，在No.734555和No.734560有POST a.php，猜测应该是webshell，让我们查找数据库的密码，直接过滤包含`database`参数，而且访问返回值是成功的

```
http contains "database" && http contains "200"
```



![](images/826a2b500ab863c44849e683f6460d31.png)

第三问：flag{e667jUPvJjXHvEUv}
