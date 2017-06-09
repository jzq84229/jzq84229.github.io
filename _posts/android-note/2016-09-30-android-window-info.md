---
layout: post
title: Android Window介绍
category: Android
tags: window
keywords:
description:
---

# Android Window介绍

## window的setFlag方法

- 设置窗体全屏
  > getWindow().setFlags(WindowManager.LayoutParams.FLAG_FULLSCREEN, WindowManager.LayoutParams.FLAG_FULLSCREEN);

- 设置窗体始终点亮
  > getWindow().setFlags(WindowManager.LayoutParams.FLAG_KEEP_SCREEN_ON, WindowManager.LayoutParams.FLAG_KEEP_SCREEN_ON);

- 设置窗体背景模糊
 > getWindow().setFlags(WindowManager.LayoutParams.FLAG_BLUR_BEHIND, WindowManager.LayoutParams.FLAG_BLUR_BEHIND);

以上都需要在setContentView方法调用之前设置
