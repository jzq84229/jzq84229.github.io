---
layout: post
title: 进程和线程
category: Android
tags: Android
keywords: process,thread
description:
---

## 进程


## 线程

Android UI 工具包并非线程安全工具包。因此，您不得通过工作线程操纵 UI，而只能通过 UI 线程操纵用户界面。因此，Android 的单线程模式必须遵守两条规则：
- 不要阻塞 UI 线程
- 不要在 UI 线程之外访问 Android UI 工具包

Android 提供了几种途径来从其他线程访问 UI 线程。以下列出了几种有用的方法：
- [Activity.runOnUiThread(Runnable)](https://developer.android.com/reference/android/app/Activity.html#runOnUiThread(java.lang.Runnable))
- [View.post(Runnable)](https://developer.android.com/reference/android/view/View.html#post(java.lang.Runnable))
- [View.postDelayed(Runnable, long)](https://developer.android.com/reference/android/view/View.html#postDelayed(java.lang.Runnable, long))

MessageQuene Looper Message ThreadLocal