---
layout: post
title: windows命令笔记
category: 笔记
tags:
keywords:
description:
---

#### Windows命令查看文件MD5
```
certutil -hashfile yourfilename.ext MD5
certutil -hashfile yourfilename.ext SHA1
certutil -hashfile yourfilename.ext SHA256
```

#### 利用无线网卡分享网络
```
1. 查看自己所创建的网络 netsh wlan show hostednetwork
2. 创建共享网络 netsh wlan set hostednetwork mode=allow ssid=acmed_jun key=1234567890
3. 启动承载网络 netsh wlan start hostednetwork
4. 关闭共享网络 netsh wlan stop hostednetwork
5. 查看所创建网络的密码 netsh wlan show hostednetwork setting=security
6. 删除共享网络 netsh wlan set hostednetwork mode=disallow
```
