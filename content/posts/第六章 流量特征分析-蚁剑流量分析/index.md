---
title: 第六章 流量特征分析-蚁剑流量分析
subtitle:
date: 2025-03-14T14:28:01+08:00
slug: 4772d92
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
1.木马的连接密码是多少
2.黑客执行的第一个命令是什么
3.黑客读取了哪个文件的内容，提交文件绝对路径
4.黑客上传了什么文件到服务器，提交文件名
5.黑客上传的文件内容是什么
6.黑客下载了哪个文件，提交文件绝对路径
```

第一问查找木马的连接密码，首先要先上传木马，或利用进行命令执行，所以上传以后或命令执行以后，`http`的返回值应该为`200`，过滤器筛选一下

```
http.response.code == 200
```

![](images/4a42172bdd12d31021c1341e8066ec65.png)
追踪一下`http流`，发现全是类似于
![](images/048297581492d2d2991ca9786e60951b.png)
所以可以判断，木马的密码为`1`
第一问flag为：flag{1}

第二问让我们查找第一个执行的命令，实际上是寻找包含字符串“200”的HTTP数据包。

```
http contains "200"
```

![](images/26eb3bea07d79678625916623070a167.png)
根据时间去查找第一个，点击查看详细，拉到最后发现，
![](images/202a8f35723c451718bda186c9ef7543.png)
有一串base64，选中右键，显示分组字节，将开始值设置为`2`
![](images/9f0fadca3d27723f7f86e993b7853a26.png)
因为一般的会在编码前添加随机字符，导致不能正常解码。
尝试提交`cd`命令，发现并不对，考虑逻辑关系，拿到shell后，通常会先确认自己的权限，所以判断首先执行`id`
第二问flag{id}

第三问问我们读取了哪个文件的内容，提交文件绝对路径。文件读取通常使用特定的http方法，`GET`或者`POST`，可以通过`http.request.method == ""`
首先我们查看一下`POST`请求的http响应吧

```
http.request.method == "POST"
```

![](images/f39d111a0eea3972ea9ed1c8705780fd.png)
一个一个查看吧
![](images/092ccbad815812784fd17c7ff73b4c5c.png)
在第三个解码
![](images/37d8770a5bd518091917fdf589504b6e.png)
第三问：flag{/etc/passwd}

第四问让我们找到上传了什么文件到服务器，提交文件名
还是第三问的步骤，继续对这个页面进行查找
![](images/c3b12a2f26700ff3cc9113fb857f4820.png)
![](images/ce2d66bc52445fd7d5f473ab3c93b785.png)
第四问：flag{flag.txt}

第五问上传的文件内容是什么。还是这个包，追踪`http流`
![](images/1e9b5c15989315586e89dba55920beb0.png)
对这段内容尝试解码
![](images/2d63a9e9140be471e8e760708cbc9cd0.png)上面的php代码，从post请求获得两个参数，第一个参数是写入文件的内容，第二个参数是传入到的路径。（注意传入路径需要去掉前两字符，消除混淆）
![](images/5629dcad735e8034b81f7ac2b22a786a.png)
第五问：flag{write_flag}

第六问下载的文件，既然成功下载，那么响应包一定是`200`

```
http contains "200"
```

![](images/66e184629f5fbd1ebdab605917a0e58b.png)
应该是最后执行的命令，所以筛选时间，
![](images/eb0af27b28cf53f9f6ec40b6ecaa524f.png)
第六问：flag{/var/www/html/config.php}
