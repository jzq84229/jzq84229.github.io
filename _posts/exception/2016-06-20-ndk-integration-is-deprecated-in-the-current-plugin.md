---
layout: post
title: Android NDK集成错误
category: Exception
tags: Android
keywords: ndk
description:
---

在集成ndk开发时，编译时提示如下错误：  
Error:(14, 1) A problem occurred evaluating project ':app'.
> Error: NDK integration is deprecated in the current plugin.  Consider trying the new experimental plugin.  For details, see http://tools.android.com/tech-docs/new-build-system/gradle-experimental.  Set "android.useDeprecatedNdk=true" in gradle.properties to continue using the current NDK integration.

解决方法：
1. 在工程根目录下添加gradle.properties文件
2. 在gradle.properties文件中添加"android.useDeprecatedNdk=true"
