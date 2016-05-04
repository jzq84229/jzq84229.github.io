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

获取任务帮助信息：

	gradlew help --task someTask

可以显示指定任务的详细信息。或者多项目构建中相同任务名称的所有任务的信息。

	gradlew -q help --task libs

获取依赖信息： 执行**gradlew dependencies**会列出项目的依赖列表，所有依赖会根据任务区分，以树形结构展示出来。

	gradlew -q dependencies app:dependencies

过滤依赖信息：  
可以通过--configuration 参数来查看 指定构建任务的依赖情况。

	gradle -q api:dependencies --configuration testCompile

查看特定依赖：  
执行 Running gradle dependencyInsight 可以查看指定的依赖情况。

	gradle -q webapp:dependencyInsight --dependency groovy --configuration compile
	
参考资料：
---

[http://stormzhang.com/devtools/2015/01/05/android-studio-tutorial5/](http://stormzhang.com/devtools/2015/01/05/android-studio-tutorial5/)  
[http://www.flysnow.org/2015/03/30/manage-your-android-project-with-gradle.html](http://www.flysnow.org/2015/03/30/manage-your-android-project-with-gradle.html)  
[http://blog.csdn.net/qinxiandiqi/article/category/2394347](http://blog.csdn.net/qinxiandiqi/article/category/2394347)  
[http://wiki.jikexueyuan.com/project/gradle/using-the-gradle-command-line.html](http://wiki.jikexueyuan.com/project/gradle/using-the-gradle-command-line.html)
