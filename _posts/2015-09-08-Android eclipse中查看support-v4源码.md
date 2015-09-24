---
layout: post
title: eclipse中查看support-v4源码
category: android
---

在libs目录下建个和jar包名字一样的properties文件，内容是src:源码路径；doc：index.html的路径。
例如：

    libs/android-support-v4.jar

创建一个新文件，名字为android-support-v4.jar.properties

    libs/android-support-v4.jar.properties

编辑android-support-v4.jar.properties

    src=/home/.../android-sdk/extras/android/support/v4/src

保存后重启eclipse。