---
layout: post
title: 学习笔记——运行时权限
category: Android
tags: permission
keywords:
description:
---

- (System Permissions)[https://developer.android.com/training/permissions/index.html?hl=zh-cn]
- (Best Practices for App Permissions)[https://developer.android.com/training/articles/user-data-permissions.html]


#### 正常权限和危险权限
系统权限分为几个保护级别。需要了解的两个最重要的保护级别是`正常权限`和`危险权限`：
- 正常权限涵盖需要访问其外部数据或资源，但对用户的隐私和其他应用操作风险很小。如：设置时区的权限就是正常权限。如果应用声明其需要的正常权限，系统会自动向应用授予权限。正常权限完整列表：(正常权限)[https://developer.android.google.cn/guide/topics/permissions/normal-permissions.html]
- 危险权限涵盖应用需要涉及用户隐私信息的数据或资源，或者可能对用户存储的数据或其他应用的操作产生影响的区域。例如，能够读取用户的联系人属于危险权限。如果应用声明其需要危险权限，则用户必须明确向应用授予该权限。
