---
layout: post
title: Gradle构建android程序
category: Android
tags: gradle
keywords: gradle
description:
---

## Introduction(简介)
### Goals of the new Build System（gradle构建系统的目标）
- 更加容易重用代码和资源。
- 更加容易创建应用程序的不同版本，无论是多个apk发布版本还是同一个应用的不同定制版本。
- 更加容易配置，扩展和定制构建过程。
- 整合优秀的IDE

### Why Gradle?(为什么使用gradle)
Gradle是一个优秀的构建系统和构建工具，它允许通过插件创建自定义的构建逻辑。   
以下是我们选择Gradle的一些原因：

- 采用了Domain Specific Language(DSL语言)来描述和控制构建逻辑。
- 构建文件基于Groovy，并且允许通过混合声明DSL元素和使用代码来控制DSL元素以控制自定义的构建逻辑。
- 支持Maven或者Ivy的依赖管理。
- 非常灵活。允许使用最好的实现，但是不会强制实现的方式。
- 插件可以提供自己的DSL和API以供构建文件使用。
- 良好的API工具供IDE集成。

## Android Studio项目目录结构
### Project结构类型
![Alt Text](http://7xn4nm.com1.z0.glb.clouddn.com/18501-35faada14c776733.png)

目录结构说明（详见[Android Project Files][2]）：

- **.gradle/**
- **.idea/** IntelliJ IDEA 的设置目录。
- **gradle/** gradle的包装文件目录。
- **build.gradle** 项目的gradle编译配置文件。项目必须包含的这个文件。
- **setting.gradle** 定义项目包含的模块。
- **gradle.properties** Gradle全局属性设置文件。
- **loacal.properties** 自定义构建系统属性，如：SDK/NDK 路径。由于此文件内容指定了本地SDK的安装路径，所以此文件不应该提交到版本控制系统。
- **gradlew** Gradle启动脚本 for Unix。
- **gradlew.bat** Gradle启动脚本 for Windows。
- **.iml** IntelliJ IDEA创建的项目的配置文件。
- **External Libraries** 项目依赖的Lib，编译时自动下载。
- **app/** 项目模块。
- **app/build/** app模块build编译输出的目录。
- **app/build.gradle** app模块的gradle编译配置文件。
- **app/app.iml** IntelliJ IDEA创建的app模块的配置文件。
- **app/proguard-rules.pro** app模块proguard文件。

## Basic Project(基本项目)
一个Gradle项目的构建过程定义在项目根目录下的build.gradle文件中。
### Simple build files(简单的构建文件)
一个最简单的Gradle纯Java项目的build。gradle文件包含以下内容：

    apply plugin 'java'

这里引入了Gradle的Java插件。这个插件提供了所有构建和测试Java应用程序所需要的东西。
最简单的Android项目的build.gradle文件包含以下内容：

    buildscript {
        repositories {
            jcenter()
        }

        dependencies {
            classpath 'com.android.tools.build:gradle:1.0.0'
        }
    }

    apply plugin: 'com.android.application'

    android {
        compileSdkVersion 19
        buildToolsVersion "19.0.0"
    }

    allprojects {
        repositories {
            jcenter()
        }
    }

- **buildscript:** 用于设置驱动构建过程的代码。
- **jcenter():** 声明使用maven仓库。在老版本中，此处为mavenCentral()。
    + **mavenCentral():** 表示依赖从Central Maven 2仓库中获取。
    + **jcenter():** 表示依赖从Bintary’s JCenter Maven 仓库中获取
    + **mavenLocal():** 表示依赖从本地的Maven仓库中获取。
- **dependencies:** 声明了使用Android Studio gradle插件版本。一般升级AS或者导入从Eclipse中生成的项目时需要修改下面的gradle版本。
- **allprojects:** 设置每一个module的构建过程。在此列中，设置了每一个module使用maven仓库依赖。
- **android:** 配置了所有android构建过程需要的参数。这也是AndroidDSL的入口点。

## 实例讲解
### settings.gradle

    include ':app'

默认的project目录下的settings.gradle文件内容入上。有可能默认情况下，project目录下的settings.gradle文件不存在，你可以自己创建。

- include ':app'： 表示当前project下有一个名称为app的module。
- 若有多个module可用逗号(,)隔开添加其他module名称。也可另一起行用include ':app1'。

如果你的module并不是project根目录下，你可以这么设置。

    include ':app2'
    project(':app2').projectDir = new File('path/to/app2')

### module的build.gradle

默认的module目录下的build.gradle文件内容如下：

    apply plugin: 'com.android.application'

    android {
        compileSdkVersion 21
        buildToolsVersion "21.1.2"

        defaultConfig {
            applicationId "cc.bb.aa.myapplication"
            minSdkVersion 10
            targetSdkVersion 21
            versionCode 1
            versionName "1.0"
        }
        buildTypes {
            release {
                minifyEnabled false
                proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
            }
        }
    }

    dependencies {
        compile fileTree(dir: 'libs', include: ['*.jar'])
        compile 'com.android.support:appcompat-v7:21.0.3'
    }

- **apply plugin: 'com.android.application'**：
表示使用 com.android.application 插件。也就是表示，这是一个 android application module 。   com.android.library 表示，这是一个 android library module 。
- **android:**: 配置所有android构建过程需要的参数。
- **compileSdkVersion**: 用于编译的SDK版本。
- **buildToolsVersion**: 用于Gradle编译项目的工具版本。
- **defaultConfig**: Android项目默认设置。
    + applicationId: 应用程序包名。
    + minSdkVersion: 最低支持Android版本。
    + targetSdkVersion: 目标版本。实际上应为测试环境下测试机的Android版本。
    + versionCode: 版本号。
    + versionName: 版本名称。
- **buildTypes**: 编译类型。默认有两个：release和debug。我们可以在此处添加自己的buildTypes，可在BuildVariants面板看到。
    + minifyEnabled: 是否使用混淆。在老版本中为runProguard， 新版本之所以换名称，是因为新版本支持去掉没使用到的资源文件，而runProguard这个名称已不合适了。
    + proguardFiles: 使用的混淆文件，可以使用多个混淆文件。此列中，使用了SDK中的proguard-android.txt文件以及当前module目录下的proguard-rules.pro文件。
- **dependencies**: 用于配置应用的依赖。
    + compile fileTree(dir: 'libs', include: ['*.jar'])： 引用当前module目录下的libs文件夹中的所有.jar文件。
    + compile 'com.android.support:appcompat-v7:21.0.3'： 引用 21.0.3版本的 appcompat-v7 （也就是常用的v7 library 项目）














参考资料

----

[Gradle Plugin User Guide][1]  
[Gradle Android插件用户指南翻译](http://avatarqing.github.io/Gradle-Plugin-User-Guide-Chinese-Verision/index.html)  
[Android 项目结构][2]  
[Studio教程](http://ask.android-studio.org/?/explore/category-studio)  







[1]: https://sites.google.com/a/android.com/tools/tech-docs/new-build-system/user-guide
[2]: https://developer.android.com/tools/projects/index.html#ProjectFiles
