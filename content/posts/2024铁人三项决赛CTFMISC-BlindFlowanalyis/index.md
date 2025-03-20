---
title: 2024铁人三项决赛CTFMISC-BlindFlowanalyis
subtitle:
date: 2025-03-14T14:49:54+08:00
slug: 02800ce
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

先分析`webflow1.pcap`，大致翻了一下，过滤一下`http`，发现`Length Info`内有`select`，猜测攻击方式为SQL注入
直接过滤含有`select`的流量即可

![](images/51f8c9751284f164ee290de77303a52f.png)



```
http && http contains "select"
```

追踪http流，

![](images/2d6dfcbb66e1062d4ac051fd8d245a0c.png)

发现在最后有admin的密钥，提取出来

```
admin

-----BEGIN PUBLIC KEY-----

MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQDBtGmqBdtXPN9TcBo9UXwHbozxSjAgOk4sfOTYhbWgjcp8ia2BaIsHUfOB2SpevVejexwNxDFOo6v/mycoFOF8wqpiii8tGTVB+FBo39YKkBI4yI863VsB3NJV8gedFZAybEUkwkFhGN7mVs9XTaYYrHXNFWRIxjvnjQtpMnAvvQIDAQAB

-----END PUBLIC KEY-----


-----BEGIN ENCRYPTED PRIVATE KEY-----

MTTC3TBXBgkqhkiG9w0BBQ0wSjApBgkqhkiG9w0BBQwwHAQIVLQmLXnlUYkCAggAMAwGCCqGSIb3DQIJBQAwHQYJYIZIAWUDBAEqBBBCQKgOObf2BrhJ9tHvAb4oBIICgP3WaNdtO3oTAX1GjdqBL0f6HvFZ9q25EIOAah78rfhKNrJQP0uHHUnbIDKUZTSMwNZ7kNQAAnJPdjwhZVNbymCjUrVvqs0VD8GWncUlIIxrAkpsgqioABSKLoy280ubg2s1IV/sqbuuvN4ldPvnAvyrLLnNLgGyU86v2Q1ArFWfG4kgeZSpW06W6TwgeIZbjRSnLKFXx1VEhpZSCOadg/HOh8BfLbFlZiWgDiuTdVSzGq34fLV8jKYfUjhEffh/tCNhjU5E8M6ItqgXZYyjbBO2ujewBzV1JiBE0QvcavwT8JvV2IDQXBtQmBJZXH4Vqoo2n5YmUgUFDxoZSdAqOfh5kcgO0OgORhOrX/qNisaDfBMwGx2csywJBuSujzZ3ckV3Gv2ysMzRPKfyZ/v10zElsrKZHci2wDMZ4XYqNyvPDafWaxkSNXP64eYRS8J6WVgWycaPq0LkoJnuxtmOnlYlZ8cmY2zOf4WfyG7X+NhK6CllS0or2Y7fuqo80dmGTrg7cuVA1GguRMA6qZAOlbqu8zT53vGUOIHDRQFb8DUU5H6t+I3e0+0Qj8ZwgTtUMCx2/WLc0YAq18Al9okrtsxuYIQ815SSxXKOXZsX93hACLEFFkKOW7vNdBMJdVJnbPoaMotkDYtH/d8TYPrrtyOhPRa/Xl8BzA9J40Z3PE+5XaQfTC4Wx9GMb3y/Dv1lVXrAd9XvCPD8jr6tu+aQWpMAq0uLpF0kgyS/5FDfIpiEHMMuRQ+FBKS58HWVkX1yKNmXkljJRpwe53AHLuDgt+0LNhry+5ck2GFH7bkxU13WmotrtUtnSdIX7FZrQzbJmbwHiyMi7oGdKT7vxkr3aFY=

-----END ENCRYPTED PRIVATE KEY-----
```

此时的`PRIVATE KEY`格式并不正确，需要把`MTTC`替换成`MIIC`，格式才正确
再去分析`webflow2.pcap`，同样的也是SQL注入，过滤条件一样

![](images/3b8cf0666b05d7a03075bffff8d60474.png)

追踪http流，发现admin的数据

```
MDgnetOpihRoTmbreC2P7EQqkmeHloAWQ0SA2gKuHWPUP3u8u81ewsTnlyhvc7qL
MMpVl36M9Z0Hu++yIKt2C/mimOFH04ixQAUo5y8h8vajw7vRLwfhpxC+pSjWvxjP
2ieWVgdmXraijq92K6vdXod/SVaOyBT/1/asqhq1abQ=
```

并拿到了提示



![](images/f6622b64ec8d2c38e30fdba8c193e27c.png)



尝试了`rockyou`字典爆破，但是好像并没有用，找不到字典，借鉴一下师傅的脚本
[第一届“长城杯”信息安全铁人三项赛线下决赛- WriteUp](https://mp.weixin.qq.com/s/p484j38nCFGpuy6qkv-hdA)

  

```
from Crypto.PublicKey import RSA
from rich.progress import Progress

# 读取加密的私钥文件
file_in = open("private.pem", "rb")
encrypted_key = file_in.read()
file_in.close()

# 密码爆破函数
def passphrase_cracker():
    # 读取密码字典文件
    with open("passwd.txt", "r", encoding="latin-1") as file:
        passwords = file.readlines()
    print(1)
    # 尝试每个密码
    with Progress() as progress:
        task = progress.add_task("Processing...", total=len(passwords))
        for password in passwords:
            passphrase = password.strip()
            progress.update(task, advance=1)
            try:
                key = RSA.import_key(encrypted_key, passphrase=passphrase)
                print(f"Passphrase cracked: {passphrase}")
                print("n = %d" % key.n)
                print("e = %d" % key.e)
                print("d = %d" % key.d)
                print("p = %d" % key.p)
                print("q = %d" % key.q)
                break  # 如果找到正确的密码，停止循环
            except ValueError:
                continue


# 调用密码爆破函数
passphrase_cracker()
```

```
from Crypto.PublicKey import RSA
import libnum
import gmpy2

n = 136024092362152689710172713899392826085562613819502637163574709012959800908069097569347489959246188000879909455954675900898187256462694001680199507890264369748020986792003039927832146357618380320485640247974651461597017754521523478582789651823653650205470697891883134131193787384293246455773119816555908050877
e = 65537
d = 96983349311172448114610684077344531859866705158061854017461211652174689366790434748659538326833645524163686606777761277160529104669611099325734812475218305914063705028257975357368955813173682894994622796568980398528404622799756620123598161913849740935593865664141149764560174893499019267153779766584891066853
p = 12048894621399454101058574170146672880023504304935502855438830209547540828334463319100866266089541071671086068945870668091434002554455832672446379382564831
q = 11289342021513486663717934560260253958877903230700176094000844663541917077757746704211638621740652499799934180688739478331856827700031454191788549757111267

c = "MDgnetOpihRoTmbreC2P7EQqkmeHloAWQ0SA2gKuHWPUP3u8u81ewsTnlyhvc7qLMMpVl36M9Z0Hu++yIKt2C/mimOFH04ixQAUo5y8h8vajw7vRLwfhpxC+pSjWvxjP2ieWVgdmXraijq92K6vdXod/SVaOyBT/1/asqhq1abQ="

import base64

c = base64.b64decode(c.encode())
c = libnum.s2n(c)

phi = (p - 1) * (q - 1)
d = gmpy2.invert(e, phi)
m = pow(c, d, n)
print(libnum.n2s(int(m)))

# flag{182w3t-he5dr4y8g-gy590-gggtd46nd-dgw3456utg676}
```



