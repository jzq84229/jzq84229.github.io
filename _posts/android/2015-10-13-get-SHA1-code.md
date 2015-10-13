---
layout: post
title: 获取KeyStore SHA1码
category: Android
tags: keystore
keywords: keystore,sha1
description:
---

keystore的SHA1，格式如：

    SHA1: 9D:66:68:DB:D1:96:39:3D:CA:5D:06:3D:BC:8B:87:34:99:7E:29:06

在cmd命令行中查询SHA1码：

1. 打开cmd命令输入框。
2. 进入keystore文件目录。以默认debug.keystore为例，在当前用户目录的.adnroid目录下。
3. 输入命令

        keytool -list -v -keystore debug.keystore
    
    根据提示输入秘钥口令(默认没有)，按回车，得到debug.keystore的签名详情。  
    ![keystore info](/assets/images/keystore-info.jpg)