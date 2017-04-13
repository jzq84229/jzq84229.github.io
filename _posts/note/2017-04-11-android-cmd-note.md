---
layout: post
title: android开发错误笔记
category: Android
tags: android
keywords:
description:
---

### android 日志抓取流程

1. 安装jdk，配置环境
2. 在E盘新建目录log(log日志存放位置)
3. 下载android sdk 安装到E:\Android-sdk\（目录自定义）
4. 打开cmd进入android sdk目录 E:\Android-sdk\android-sdk-windows\platform-tools>
5. 用usb线连接电脑和设备
6. 在cmd命令中输入：adb logcat -v time > E:\log\log1.txt
7. 抓取结束后按Ctrl+C结束命令
