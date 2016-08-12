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
