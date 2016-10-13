---
layout: post
title: adb命令
category: 笔记
tags:
keywords:
description:
---

### adb命令
adb shell pm list packages：列出所有的包名。  
adb shell dumpsys package：列出所有的安装应用的信息  
dumpsys package com.android.XXX：查看某个包的具体信息  

#### 查看logcat
查看和跟踪系统日志缓冲区的命令logcat的一般用法是：

```
[adb] logcat [<option>] ... [<filter-spec>] ...
```

#### adb查看cpu信息
```
adb shell
cd proc
cat cpuinfo
```

#### adb无线连接调试app
1. 使用usb连接手机
2. 输入命令：adb tcpip 5555 系统会返回`restarting in TCP mode port:5555`
3. 查看手机ip如：192.168.0.120 输入命令：`adb connect 192.168.0.120:5555` 系统返回:`adb connect 192.168.0.142:5555`
手机已通过wifi连接成功，可以拔掉usb即可使用adb命令
4. 若要退出无线模式，输入：adb usb
