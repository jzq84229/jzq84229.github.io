---
layout: post
title: 学习笔记——rxjava笔记
category: 笔记
tags: rxjava
keywords:
description:
---


### doOnXXX
使用这一组操作符，我们能在使用简单Consumer回调对onNext方法作处理时，依然能对其他三种回调做处理：

- doOnSubscribe
在onNext之前做初始化操作。

- doOnNext
允许我们在每次输出一个元素之前做一些额外的事情。

- doOnError
当出现错误时复写方法做处理

- doOnComplete
当onNext执行完时，做最后总处理
