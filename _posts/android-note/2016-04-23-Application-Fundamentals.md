---
layout: post
title: Android 应用基础
category: Android
tags: Android
keywords:
description:
---

# android应用基础
原文地址：[https://developer.android.com/guide/components/fundamentals.html](https://developer.android.com/guide/components/fundamentals.html)

通常android应用使用java语言编写。android SDK会将你的代码和资源文件编译为一个后缀名为.apk的APK压缩文件。
APK文件包含android应用的所有内容，他是android设备安装应用的文件。  
一旦app安装到android设备，每一个app都在运行在自己的安全沙盒内：

 - android是一个多用户的linux操作系统，每个app就是一个不同的用户。
 - 系统会默认给每一个app一个唯一的linux user ID（这个ID只能有系统使用，app自己并不知道）。系统为app的所有文件设置权限，只有这个特定app可以操作这些文件。
 - 每个进程都有它独立的虚拟机（VM），所以app之间是完全隔离的。
 - 每个app默认运行在一个独立的linux进程中。当app的任何一个组件被需要执行是，android会启动应用的进程。当不在需要这个应用或系统需要为其他应用回收内存时关闭这个进程。

Android系统可以通过这种方式实现最小权限原则。也就是说，默认情况下，每个应用只能访问执行其工作所需的组件，而不能访问其他组件。
这样便营造出一个非常安全的环境，在这个环境中，应用无法访问系统中其未获得权限的部分。
不过，应用仍然可以通过一些途径与其他应用共享数据及访问系统服务：

  - 可以安排两个应用共享同一Linux用户ID，在这种情况下，它们能够相互访问彼此的文件。为了节省系统资源，可以安排具有相同用户ID的应用在同一Linux进程中运行，并共享同一VM（应用还必须使用相同的证书签名）；
  - 应用可以请求访问设备数据（如用户的联系人、短信、可装入存储装置[SD卡]、相机、蓝牙等）的权限。所有应用权限都必须由用户在安装时授予。

应用组件
---
应用组件是android应用的基本构建基块。每个组件都是一个不同的点，系统可以通过它进入您的应用。
并非所有组件都是用户实际入口点，有些组件相互依赖，但每个组件都一独立实体形式存在，并发挥特定作用。每个组件都是唯一的构建基块，有助于定义应用的总体行为。
共有四种不同的应用组件类型。每种类型都服务与不同的目的，并且具有定义组件的创建和销毁方式的不同生命周期。  
这四种应用组件是：Activity、Service、Content provider、Broadcast receiver

启动组件
---
四种组件类型种的三种：Activity、Service和Broadcast receiver通过名为Intent的异步消息进行启动。
Intent会在运行时将各个组件相互绑定（您可以将Intent看做从其他组件请求操作的信使），无论组件属于您的应用还是其他应用。  
Intent可以显式声明，也可以隐式声明。隐式Intent的作用无非是描述要执行的操作类型（还可选择描述您想执行的操作所针对的数据）， 让系统能够在设备上找到科执行该操作的组件，并启动该组件。
如果有多个组件可以执行Intent所描述的操作，则由用户选择使用哪一个组件。  
如果触发了一个intent，而没有任何一个app会去接收这个intent，则app会crash。
所以在发送隐式intent时请对 Intent 对象调用 resolveActivity()，如果结果为非空，则至少有一个应用能够处理该 Intent，且可以安全调用；否则不应使用该 Intent。如：

    // Create the text message with a string
    Intent sendIntent = new Intent();
    sendIntent.setAction(Intent.ACTION_SEND);
    sendIntent.putExtra(Intent.EXTRA_TEXT, textMessage);
    sendIntent.setType("text/plain");

    // Verify that the intent will resolve to an activity
    if (sendIntent.resolveActivity(getPackageManager()) != null) {
        startActivity(sendIntent);
    }

或在发送隐式intent时需要执行queryIntentActivities()来获取到能够接收这个intent的所有activity的list。只有返回的list非空，我们才可以安全的使用这intent。如：

    PackageManager packageManager = getPackageManager();
    List<ResolveInfo> activities = packageManager.queryIntentActivities(intent, 0);
    boolean isIntentSafe = activities.size() > 0;

每种类型的组件有不同的启动方法：

  - 启动Activity， 可用通过将Intent传递到startActivity()或startActivityForResult()（当想让activity返回结果时）
  - 启动Service， 可以通过将Intent传递到startService()来启动服务（或对执行中的服务下达新指令）。或者也可以通过将Intent传递到bindService()来绑定到该服务；
  - 发送BroadCast， 可以通过将Intent传递到sendBroadcast()、sendOrderedBroadcast()或sendStickyBroadcast()等方法来发起广播。
  - 可以通过在ContentResolver上调用query()来对内容提供程序执行查询。
  
如需了解有关 Intent 用法的详细信息，请参阅[Intent 和 Intent 过滤器](https://developer.android.com/guide/components/intents-filters.html)文档。 
以下文档中还提供了有关启动特定组件的详细信息： 
[Activity](https://developer.android.com/guide/components/activities.html)、
[服务](https://developer.android.com/guide/components/services.html)、
[BroadcastReceiver](https://developer.android.com/reference/android/content/BroadcastReceiver.html) 和
[内容提供程序](https://developer.android.com/guide/topics/providers/content-providers.html)。

清单文件
---
在android系统启动应用前，系统必须通过读取应用的AndroidManifest.xml文件("清单"文件)确认组件存在。您的应用必须在此文件中声明其所有组件，该文件必须位于应用项目的根目录中。  
除了声明应用的组件，清单文件还有其他作用：

  - 确定应用需要的任何用户权限，如：互联网访问权限或用户联系人读取权限
  - 根据应用使用的API，声明应用所需的最低API级别。
  - 声明应用使用或需要的硬件和软件功能，如相机、蓝牙服务或多点触摸屏幕。
  - 应用需要链接的API库（Android框架API除外），如Google Maps API库。
  - 其他功能
  
您必须通过以下方式声明所有应用组件：
  - Activity的[&lt;activity>](https://developer.android.com/guide/topics/manifest/activity-element.html)元素
  - 服务的[&lt;service>](https://developer.android.com/guide/topics/manifest/service-element.html)元素
  - 广播接收器的[&lt;receiver>](https://developer.android.com/guide/topics/manifest/receiver-element.html)元素
  - 内容提供程序的[&lt;provider>](https://developer.android.com/guide/topics/manifest/provider-element.html)元素
  
您包括在源代码中和高，但未在清单文件中声明的Activity、服务和内容提供程序对系统不可见，因此也永远不会运行。
不过，广播接收器可以在清单文件中声明或在代码中动态创建（如Broadcast对象）并通过调用registerReceiver()在系统中注册。
构建清单文件的详细信息，请参阅[AndroidManifest.xml文件](https://developer.android.com/guide/topics/manifest/manifest-intro.html)文档。
