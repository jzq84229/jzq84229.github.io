---
layout: post
title: Gradle 常用资源
category: 资源
tags: gradle
keywords: gradle
description:
---



- 查看gradle版本

		gradlew -v

	如果第一次执行会下载gradle。

- 查看gradle可用任务命令

		gradlew tasks

- 清空项目的所有输出

		gradlew clean

	这个命令会去下载gradle的一下依赖，下载成功并编译通过时会看到

	> BUILD SUCCESSFUL

- 检查依赖并编译打包

		gradlew build

	这个命令会直接编译并生成相应的apk文件，如果build成功会看到

	> BUILD SUCCESSFUL
	这个命令会把signingConfigs中环境的包全都打出来。

- 编译并打Debug包

		gradlew assembleDebug

- 编译并打Release包

		gradlew assembleRelease

- Release模式打包并安装

		gradlew installRelease

- 卸载Release模式包

		gradlew uninstallRelease

如果执行命令时提示错误：

> Task not found in root project 'app'

可用：

	gradlew tasks

查看gradlew可用任务命令


参考资料：
---

[http://stormzhang.com/devtools/2015/01/05/android-studio-tutorial5/](http://stormzhang.com/devtools/2015/01/05/android-studio-tutorial5/)  
[http://www.flysnow.org/2015/03/30/manage-your-android-project-with-gradle.html](http://www.flysnow.org/2015/03/30/manage-your-android-project-with-gradle.html)  
[http://blog.csdn.net/qinxiandiqi/article/category/2394347](http://blog.csdn.net/qinxiandiqi/article/category/2394347)
