---
layout: post
title: Android 系统架构
category: Android
tags: Android
keywords:
description:
---

# android系统架构
原文地址：[https://source.android.com/devices/index.html](https://source.android.com/devices/index.html)

![android-architecture](../../assets/images/ape_fwk_all.png)


Application Framework
---
Application Framework是app开发人员最经常使用的。你必须了解所有开发人员用到的API，与HAL接口是一一对应的，并提供有用的信息去实现驱动。

Binder IPC
---
Binder Inter-Process Communication(IPC)内部进程通讯机制允许应用框架通过进程边界来调用Android系统的服务代码。使顶层的框架API能够与Android系统服务进行交互。在application framework层级，这种通信对application framework层级的开发人员是不可见的仅显示为“就这样做”；

Android System Services
---
application framework API暴露的函数与系统服务调用的底层硬件通信。服务是模块化，聚焦的组件如Window Manager， Search Service或Notification Manager。
android包含两种服务：system（如Window Manager及Notification Manager服务）和media（涉及媒体播放及录制的服务）

HAL
---
Hardware Abstraction Layer硬件抽象层为硬件厂商定义了标准接口且不需要知道底层驱动的实现。HAL让你在不影响高层系统是去实现函数。HAL的实现被打包在模块文件中（.so），并且系统会在适当的时机去加载他。

Linux Kernel
---
开发android设备驱动与开发Linux设备驱动是一样的。android使用一个特定的linux内核版本，它包含wake locks（一个内存管理系统），Binder IPC驱动和其他一些对移动嵌入平台比较重要的特征。这些主要的系统功能不会影响驱动的开发。你可以使用支持的内核版本。但我们还是建议你使用最新的android内核。具体参考内核编译。
