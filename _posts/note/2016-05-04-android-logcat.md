---
layout: post
title: Android 个人笔记
category: Android
tags:
keywords:
description:
---

## android Log等级

在Android中支持六种Log类型，分别为Verbose，Info，Debug，Warn，Error和Assert。

  1. **Verbose** 详细信息，Verbose英文含义是冗长的，啰嗦的。Verbose用来记录不重要的，一般的信息，通常不需要关注。
  2. **Info** 通告信息，通常记录一些需要用户关注的消息，重要程度比Verbose高。
  3. **Warn** 警告信息，记录警告信息，这类信息通常表示应用执行过程中出现了一些问题，这些问题并不会导致整个应用崩溃，但可能会导致一些业务不能正常执行，因此需要用户重点关注，其重要程度比Info高。
  4. **Error** 错误信息，表示应用执行时出现无法处理的严重错误，通常会导致程序无法继续运行，业务中断等严重故障，需要由用户处理，其重要程度比Warn高。
  5. **Assert** 断言信息，表示断言失败后的错误消息，这类错误原本是不可能出现的错误，现在却出现了，是极其严重的错误类型。

  > Verbose，Info，Warn，Error和Assert五类Log的重要程度排序如下。  
    Assert > Error > Warn > Info > Verbose

  6. **Debug** 类型没有重要程度的含义，它表示应用的调试信息。

## Logcat对应色值

|Log级别|	色值|
|:---|:---|
|VERBOSE	|BBBBBB|
|DEBUG	|0091EA|
|INFO	|4CAF50|
|WARN	|FFAB00|
|ERROR	|F44336|
|ASSERT	|FF6B68|

![](http://winy.eicp.net/logcat.png)


参考资料：
---
[http://blog.csdn.net/ccpat/article/details/46363137](http://blog.csdn.net/ccpat/article/details/46363137)
