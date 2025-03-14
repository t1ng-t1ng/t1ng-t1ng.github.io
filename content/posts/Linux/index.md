---
title: Linux
subtitle:
date: 2025-03-14T12:10:07+08:00
slug: caff0cb
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
  - Linux
categories:
  - Linux
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

### Linux基础知识
#### 第一部分
echo    输出我们提供的任何文本
whoami    找出我们当前以哪个用户身份登录
想要输出文本“ **TryHackMe** ”，我们的命令是什么：echo TryHackMe

ls    清单
cd    进入/更改目录
cat    连接/列出文件内容
pwd    打印工作目录

find    搜索文件    
finb -name password.txt （已知文件名）
find -name *.txt （已知文件后缀）

grep    在文件内容中搜索正在寻找的特定值
wc    计算条目
grep "81.143.211.90" access.log （使用“grep”在“access.log”中查找 IP 地址为“81.143.211.90”的任何条目）
wc -l access.log （使用“wc”计算“access.log”中的条目数）

Shell运算符
&    该操作符允许您在终端后台运行命令。
&&    运算符允许您在终端的一行中将多个命令组合在一起。
'>'    操作符是一个重定向器 - 这意味着我们可以获取命令的输出（例如使用 cat 来输出文件）并将其指向其他地方。
'>>'    该运算符执行与运算符相同的功能`>`，但附加输出而不是替换（意味着没有覆盖任何内容）
command1 && command2 只有在成功command1时command2才会运行
`echo hey > welcome`在想要创建文件的地方运行，内容为“hey”    echo hey > welcome    cat welcome    hey
echo hello >> welcome    cat welcome             hey hello

#### 第二部分
ssh访问linux机器    ssh 用户名@Machine_IP

touch    创建文件
mkdir    创建文件夹
cp    复制文件或文件夹
mv    移动文件或文件夹
rm    删除文件或文件夹
file    确定文件的类型
rm 删除文件  rm -R删除文件夹
将“note”复制到“note2”。cp note note2
文件“note2”重命名为“note3” mv note2 note3
确定文件类型 file note
su user2 与登录系统的实际用户更相似的 shell - 我们继承了新用户的更多属性，即环境变量等
su -l user2带入前一个用户的主目录

公用目录
/etc    存储操作系统使用的系统文件的常用位置。
/var    存储系统上运行的服务或应用程序经常访问或写入的数据。
/root    是“root”系统用户的家。
/tmp    目录是“临时”的缩写，它是易失性的，用于存储只需要访问一两次的数据。与计算机上的内存类似，一旦计算机重新启动，此文件夹的内容就会被清除。进行渗透测试来说，任何用户默认都可以写入此文件夹。

#### 第三部分
vim nano
Wget（下载文件）
此命令允许我们通过 HTTP 从 Web 下载文件——就像您在浏览器中访问文件一样。我们只需提供要下载的资源的地址即可    wget https://assets.tryhackme.com/additional/linux-fundamentals/part3/myfile.txt

从主机传输文件 - SCP（SSH）

远程系统的 IP 地址192.168.1.30    远程系统上的用户Ubuntu    本地系统上的文件名称important.txt     希望在远程系统上存储文件的名称 transferred.txt
将示例文件从我们的机器复制到远程机器 scp important.txt ubuntu@192.168.1.30:/home/ubuntu/transferred.txt

远程系统的 IP 地址192.168.1.30    远程系统上的用户Ubuntu    远程系统上的文件名称documents.txt    希望在系统上存储文件的名称notes.txt
用于从未登录的远程计算机复制文件 的语法    scp ubuntu@192.168.1.30:/home/ubuntu/documents.txt notes.txt

ps查看进程
要终止 PID 1337，我们将使用`kill 1337`
让进程/服务在启动时启动  systemctl [option] [service] [service]  
option:Star ,Stop ,Enable ,Disable

cron进程
Crontab 是在启动期间启动的进程之一，负责促进和管理 cron 作业。
crontab 需要 6 个特定值：
MIN    在什么时候执行
HOUR    什么时间执行
DOM    在每月的哪一天执行
MON    在一年中的哪个月执行
DOW    在星期几执行
CMD    将要执行的实际命令
以备份文件为例。您可能希望每 12 小时备份一次“cmnatic”的“文档”。我们将使用以下格式：
0 */12 * * * cp -R /home/cmnatic/Documents /var/backups/ 
不关心它在哪月、哪日或哪年执行 — — 只关心它每 12 小时执行一次，我们只需放置一个星号即可

包管理apt
下载GPG密钥并使用 apt-key 来信任它wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -
更新 apt 以识别此新条目apt update。继续安装我们信任并添加到 apt 的软件，使用 `apt install sublime-text`
删除软件包就像逆向操作一样简单。这个过程可以通过使用命令`add-apt-repository --remove ppa:PPA_Name/ppa`或手动删除我们之前添加的文件来完成。删除后，我们只需使用`apt remove [software-name-here]`ie`apt remove sublime-text`

日志
三个服务的一些日志：
- Apache2 Web 服务器
- fail2ban 服务的日志，用于监控暴力破解尝试，例如
- 用作防火墙的 UFW 服务
两种类型的日志文件值得关注：
- 访问日志
- 错误日志
