---
layout: post
title: Android6.0覆盖安装错误
category: Android
tags: Android
keywords: android
description:
---


问题描述：
    **android系统6.0.1**
    app versionCode为1，使用最新版targetSdkVersion-23打包安装，
    修改versionCode为2，使用targetSdkVersion-22打包，进行覆盖安装，始终提示“应用未安装”。若要安装必须卸载之前安装targetSdkVersion-23打包的版本
    versionCode为2，使用targetSdkVersion-23打包，进行覆盖安装，安装成功。

    **android系统5.0.2**
    app versionCode为1，使用targetSdkVersion-23打包安装，
    app修改versionCode为2，使用targetSdkVersion-22打包，进行覆盖安装，安装成功。
    versionCode2,使用targetSdkVersion-23打包，进行该安装，安装成功。


    由此可知，android6.0对targetSdkVersion已设置为23的app，无法覆盖安装targetSdkVersion较低的版本,必须卸载后重新安装。  
    但在官方的文档我未能找到相关的内容。
