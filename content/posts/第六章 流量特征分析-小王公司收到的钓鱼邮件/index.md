---
title: 第六章 流量特征分析-小王公司收到的钓鱼邮件
subtitle:
date: 2025-03-14T14:35:11+08:00
slug: ebb6f8f
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
服务器场景操作系统 None
服务器账号密码 None None
下载数据包文件 hacker1.pacapng，分析恶意程序访问了内嵌 URL 获取了 zip 压缩包，该 URL 是什么将该 URL作为 FLAG 提交 FLAG（形式：flag{xxxx.co.xxxx/w0ks//?YO=xxxxxxx}） (无需 http、https)；
下载数据包文件 hacker1.pacapng，分析获取到的 zip 压缩包的 MD5 是什么 作为 FLAG 提交 FLAG（形式：flag{md5}）；
下载数据包文件 hacker1.pacapng，分析 zip 压缩包通过加载其中的 javascript 文件到另一个域名下载后续恶意程序， 该域名是什么?提交答案:flag{域名}(无需 http、https)
```

打开文件过滤`http`
![](images/7ec2543d240be4e76539382053c2fc59.png)
第一问：flag{tsdandassociates.co.sz/w0ks//?YO=1702920835}

还是追踪http，
![](images/931cce8721239d9636a2cf95a07cc35a.png)
典型的zip压缩包开头，导出
![](images/ce1eaa00a367aa7513b91a35a1dd6e9f.png)
![](images/bbf677197b9b6bf48a4fdccaa191983f.png)
第二问 ：flag{F17DC5B1C30C512137E62993D1DF9B2F}

加载js文件
![](images/f41c8f49d5105aa4fecebc1eac01df21.png)
发现大量注释，发现`o457607380`参数后面的就是域名
![](images/b13f9d083829155b33950faa7a5f0759.png)
查找拼接一下
第三问：flag{shakyastatuestrade.com}
