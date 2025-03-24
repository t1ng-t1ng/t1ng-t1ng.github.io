---
title: 2024铁人三项决赛CTFMISC-Zip_guessinteger
subtitle:
date: 2025-03-14T14:52:54+08:00
slug: 62470aa
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
ZIP包竟然加密了，快上我的暴力破解工具。难道我需要一个量子算力吗？能否使点巧力猜猜？
```

看到了`store`压缩方式，猜测需要明文攻击，而且压缩包名字较长大于12字符

```
echo -n 'breakthroughentry.txt' > 1.txt
```

bkcrack攻击部分明文，将明文`breakthroughentry.txt`写到1.txt中，偏移值为`30`，进行攻击



![](images/403ae6da5151acd9a07ec85d51df2f46.png)



```
./bkcrack -C zip_guessinteger.zip -c breakthroughentry.txt+flag.txt.zip -p 1.txt -o 30
```



![](images/150d4f3928cc5750a776975e0e07c0cc.png)

拿到key创建相同内容的压缩包

```
./bkcrack -C zip_guessinteger.zip -k 003e5ac3 885a9927 00c436d9 -U 123.zip
```



![](images/b2133c34090fdaad4d8173a9274a4476.png)

让我们研究`guessinteger`文件，在linux下缺少动态链接库，使用IDA看一下



![](images/e98d3b592903b6353bb1ba716852ab68.png)

创建了一个名为 `breakthroughentry.txt` 的文件，并向其中写入一些鼓励性的消息。
直接拿出来，注意`/r/n`

```
Hi.boys and girs. good luck for you!
But this file contains no flag,you need to try more other methods.
but this file is a great milestone for your success!
Best wishes for you.

```

明文使用360压缩zip

![](images/c2a978b45a6a75ef8d771c6937629c5e.png)

拿到flag

![](images/7195efe5b1bb69f58a868634fa539821.png)



