---
layout: post
title: 学习笔记——android组件化开发笔记
category: 笔记
tags: retrofit
keywords:
description:
---

组件化开发中遇到的问题：
### gradle配置
1. apply plugin类型冲突
build.gradle的插件类型配置:
Application module时的插件类型:
```
apply plugin: 'com.android.application'
```
Library module插件类型：
```
apply plugin: 'com.android.library'
```
这两种类型不能同时设置。为了便于两种状态切换，可在gradle.properties设置一个变量 `isModule=true`是否组件开发模式，
然后在我们组件的build.gradle获取这个变量值。  
```
if(isModule.toBoolean()){
  apply plugin: 'com.android.application'
} eles{
  apply plugin: 'com.android.library'
}
```
2. ApplicationId在Library内无法识别
这个问题解决方法同1, 只需判断是否为组件模式。
```
if(isModule.toBoolean()){
  applicationId 'com.sample.demo'
}
```
3. applicationVariants在library无法识别  
解决方法同1。
4. 默认情况下library只发布其release版本。


### library与Application的AndroidManifest.xml文件不一样


### 资源名称冲突

### 引入包冲突
