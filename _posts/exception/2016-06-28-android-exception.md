---
layout: post
title: android开发错误笔记
category: Android
tags: android
keywords:
description:
---

1. `build-tools/22.0.1/aapt finished with non-zero exit value 127`  
使用 `./gradlew assembleDebug --info` 命令找到具体出错原因  
`/opt/local/android-sdk/android-sdk-linux/build-tools/22.0.1/aapt: error while loading shared libraries: libstdc++.so.6: cannot open shared object file: No such file or directory`  
原因是: 64位的Ubuntu系统, 应该是缺少 lib32stdc++6这个包，用 `apt-get install lib32stdc++6`安装一下就可以

2. `chromium: [INFO:CONSOLE(107)] "TypeError: Cannot read property 'getItem' of null`  
This seems to allow the browser to store a DOM model of the page elements, so that Javascript can perform operations on it.  
解决方法：
```
WebSettings settings = webView.getSettings();
settings.setDomStorageEnabled(true);
```
