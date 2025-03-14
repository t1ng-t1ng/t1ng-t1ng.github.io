---
title: 第二章 日志分析-mysql应急响应
subtitle:
date: 2025-03-14T14:31:46+08:00
slug: 450ee0b
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
mysql应急响应 ssh账号 root  密码 xjmysql
ssh env.xj.edisec.net  -p xxxxx
1.黑客第一次写入的shell flag{关键字符串} 
2.黑客反弹shell的ip flag{ip}
3.黑客提权文件的完整路径 md5 flag{md5} 注 /xxx/xxx/xxx/xxx/xxx.xx
4.黑客获取的权限 flag{whoami后的值}
```

首先了解一下mysql的日志位于`/var/log/mysql`下

写入的shell位置应该为于`/var/www/html`，shell的关键词应该为`eval`

```
find ./ -name "*.php" | xargs grep "eval"
```

发现sh.php文件，cat查看一下
![](images/51ff147b944eb8a82aeb1861fca0281f.png)
第一问：flag{ccfda79e-7aa1-4275-bc26-a6189eb9a20b}

进入`/var/log/mysql`位置，
![](images/bc11bfb58613e0263d53bc970771ecc7.png)
发现只有一个错误文件，查看一下
![](images/3b1b782347e6c39b8b19d9915da9b823.png)
发现在`/tmp`目录下还有一个`1.sh`文件，切换到`/tmp`目录查看一下
![](images/beecca32f51555aad9cee9e1854ee1ed.png)
找到了反弹shell的ip地址
第二问：flag{192.168.100.13}

既然用户已经进行了提权，首先考虑mysql的账号密码是否泄露，尝试在`/var/www/html`目录下找一下

```
find ./ -name "*.php" | xargs grep "root"
```

![](images/a5b57034955037ee97fd12e25ed6581f.png)连接数据库

```
mysql -uroot -p334cc35b3c704593
```

首先检查一下是否是UDF提权，UDF提权的条件是`secure_file_priv`的值为空，检查一下环境变量

```
show global variables like 'secure_file_priv';
```

![](images/da31f11f3dbc2e9c7e327c50c416e275.png)确认是UDF提权，UDF提权会在`/usr/lib/mysql/plugin`位置留下`def.so`文件，检查一下是否有
![](images/221804ac859288709b9681d2883ac7f7.png)
得到完整路径，计算一下md5

```
echo -n '/usr/lib/mysql/plugin/udf.so' | md5sum
```

第三问：flag{b1818bde4e310f3d23f1005185b973e7}

黑客进行了提权会写入自定义函数，再使用自定义函数进行命令执行，可知获得的权限
由上面的`error.log`日志可知，写入了`mysql.func`中
![](images/2b476604a835475455f21efa71c9f507.png)

```
mysql -uroot -p334cc35b3c704593
select* from mysql.func;
select sys_eval('whoami');
```

![](images/365782e312009c879e556316fb5df84d.png)
第四问：flag{mysql}
