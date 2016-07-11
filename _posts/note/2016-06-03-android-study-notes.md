---
layout: post
title: android学习笔记
category: Android
tags: note
keywords:
description:
---

- [公共技术点之 View 事件传递](http://a.codekk.com/detail/Android/Trinea/%E5%85%AC%E5%85%B1%E6%8A%80%E6%9C%AF%E7%82%B9%E4%B9%8B%20View%20%E4%BA%8B%E4%BB%B6%E4%BC%A0%E9%80%92)  
- [公共技术点之依赖注入](http://a.codekk.com/detail/Android/%E6%89%94%E7%89%A9%E7%BA%BF/%E5%85%AC%E5%85%B1%E6%8A%80%E6%9C%AF%E7%82%B9%E4%B9%8B%E4%BE%9D%E8%B5%96%E6%B3%A8%E5%85%A5)  
- [公共技术点之 View 绘制流程](http://a.codekk.com/detail/Android/lightSky/%E5%85%AC%E5%85%B1%E6%8A%80%E6%9C%AF%E7%82%B9%E4%B9%8B%20View%20%E7%BB%98%E5%88%B6%E6%B5%81%E7%A8%8B)  
- [公共技术点之 Java 注解 Annotation](http://a.codekk.com/detail/Android/Trinea/%E5%85%AC%E5%85%B1%E6%8A%80%E6%9C%AF%E7%82%B9%E4%B9%8B%20Java%20%E6%B3%A8%E8%A7%A3%20Annotation)  
- [公共技术点之 Java 反射 Reflection](http://a.codekk.com/detail/Android/Mr.Simple/%E5%85%AC%E5%85%B1%E6%8A%80%E6%9C%AF%E7%82%B9%E4%B9%8B%20Java%20%E5%8F%8D%E5%B0%84%20Reflection)  
- [公共技术点之 Java 动态代理](http://a.codekk.com/detail/Android/Caij/%E5%85%AC%E5%85%B1%E6%8A%80%E6%9C%AF%E7%82%B9%E4%B9%8B%20Java%20%E5%8A%A8%E6%80%81%E4%BB%A3%E7%90%86)  
- [公共技术点之 Android 动画基础](http://a.codekk.com/detail/Android/lightSky/%E5%85%AC%E5%85%B1%E6%8A%80%E6%9C%AF%E7%82%B9%E4%B9%8B%20Android%20%E5%8A%A8%E7%94%BB%E5%9F%BA%E7%A1%80)  

- [EventBus 源码解析](http://a.codekk.com/detail/Android/Trinea/EventBus%20%E6%BA%90%E7%A0%81%E8%A7%A3%E6%9E%90)  
- []()  
- []()
- [Android系统服务](http://blog.csdn.net/freshui/article/details/5993195)
- [View属性说明](http://www.cnblogs.com/skywang12345/archive/2013/06/15/AndroidAttr.html)

### 代办事项
- [ ] okHttp
- [ ] rxJava
- [ ] zxing
- [ ] CharSequence和String、SpannableString笔记
- [ ] ViewDragHelper分析 [参考资料](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2014/0911/1680.html)
- [ ] Drawable与Bitmap
- [ ] Character笔记
- [ ] emoji开源框架[emojicon](https://github.com/rockerhieu/emojicon)

### adb命令
adb shell pm list packages：列出所有的包名。  
adb shell dumpsys package：列出所有的安装应用的信息  
dumpsys package com.android.XXX：查看某个包的具体信息  


### Android系统服务
[原文 http://blog.csdn.net/freshui/article/details/5993195](http://blog.csdn.net/freshui/article/details/5993195)
#### System_Server进程

运行在system server进程中的服务比较多，这是整个android框架的基础

#### Native服务

- **SurfaceFlinger**
这是framebuffer合成的服务，将各个应用程序及应用程序中的逻辑窗口图像数据(surface)合成到一个物理窗口中显示（framebuffer）的服务程序

#### Java服务：

这部分的服务大部分都有一个供应用进程使用的manager类，这就是一个RPC调用，用户通过调用xxxManager的方法，实际上被Binder给迁移到system_server进程中对应的xxxManagerService中对应的方法，并将结果再通过binder带回。


1. **EntropyService**
熵服务，周期性的加载和保存随机信息。主要是linux开机后，/dev/random的状态可能是可预知的，这样一些需要随机信息的应用程序就可能会有问题。这个无需提供应用程序接口。
2. **PowerManagerService –> PowerManager**
Android 的电源管理也是很重要的一部分。比如在待机的时候关掉不用的设备，待机时屏幕和键盘背光的关闭，用户操作的时候该打开多少设备等等。
3. **ActivityManagerService->ActivityManager**
这个是整个Android framework框架中最为核心的一个服务，管理整个框架中任务、进程管理, Intent解析等的核心实现。虽然名为Activity的Manager Service，但它管辖的范围，不只是Activity，还有其他三大组件，和它们所在的进程。也就是说用户应用程序的生命管理，都是由他负责的。
4. **TelephonyRegistry->TelephonyManager**
电话注册、管理服务模块，可以获取电话的链接状态、信号强度等等。<可以删掉，但要看的大概明白>
5. PackageManagerService -> PackageManager
包括对软件包的解包，验证，安装以及升级等等，对于我们现在不能安装.so文件的问题，应该先从这块着手分析原因。
6. **AccountManagerService -> AccountManager**
A system service that provides  account, password, and authtoken management for all
 accounts on the device。
7. **ContentService -> ContentResolver**
内容服务，主要是数据库等提供解决方法的服务。
8. **BatteryService**
监控电池充电及状态的服务，当状态改变时，会广播Intent
9. **HardwareService**
一般是ring和vibrate的服务程序
10. **SensorService -> SensorManager**
管理Sensor设备的服务，负责注册client设备及当client需要使用sensor时激活Sensor
11. **WindowManagerService -> WindowManager -> PhoneWindowManager**
和ActivityManagerService高度粘合
窗口管理，这里最核心的就是输入事件的分发和管理。
12. **AlarmManagerService -> AlarmManager**
闹钟服务程序
13. **BluetoothService -> BluetoothDevice**
蓝牙的后台管理和服务程序
14. **StatusBarService -> StatusBarManager**
负责statusBar上图标的更新、动画等等的服务，服务不大。
15. **ClipboardService -> ClipboardManager**
和其他系统的clipBoard服务类似，提供复制黏贴功过。
16. **InputMethodManagerService -> InputMethodManager**
输入法的管理服务程序，包括何时使能输入法，切换输入法等等。
17. **NetStatService**
手机网络服务
18. **ConnectivityService -> ConnectivityManager**
网络连接状态服务，可供其他应用查询，当网络状态变化时，也可广播改变。
19. **AccessibilityManagerService-> AccessibilityManager**
这块可能要仔细看一下，主要是一些View获得点击、焦点、文字改变等事件的分发管理，对整个系统的调试、问题定位等，也需要最这个服务仔细过目一下。
20. **NotificationManagerService -> NotificationManager**
负责管理和通知后台事件的发生等，这个和statusbar胶黏在一起，一般会在statusbar上添加响应图标。用户可以通过这知道系统后台发生了什么事情。
21. **MountService**
磁盘加载服务程序，一般要和一个linux daemon程序如vold/mountd等合作起作用，主要负责监听并广播device的mount/unmount/bad removal等等事件。
22. **DeviceStorageMonitorService**
       监控磁盘空间的服务，当磁盘空间不足10%的时候会给用户警告
23. **LocationManagerService -> LocationManager**
       要加入GPS服务等，这部分要细看，现在应用中的navigation没响应，可以从此处着手看一下
24. **SearchManagerService -> SearchManager**
The search manager service handles the search UI, and maintains a registry of searchable activities.
25. **Checkin Service(FallbackCheckinService)**
貌似checkin service是google提供的包，没有源代码，源码只有fallbackCheckinService
26. **WallpaperManagerService -> WallpaperManager**
管理桌面背景的服务，深度定制化桌面系统，需要看懂并扩展<同时要兼容>这部分
27. **AudioService -> AudioManager**
AudioFlinger的上层管理封装，主要是音量、音效、声道及铃声等的管理
28. **HeadsetObserver**
耳机插拔事件的监控小循环
29. **DockObserver**
如果系统有个座子，当手机装上或拔出这个座子的话，就得靠他来管理了
30. **BackupManagerService -> BackupManager**
备份服务
31. **AppWidgetService -> AppWidgetManager**
Android可以让用户写的程序以widget的方式放在桌面上，这就是这套管理和服务的接口
32. **StatusBarPolicy**
管理哪个图标该在status bar上显示的策略。


#### mediaServer服务进程

MediaServer服务基本上都是native的services，mediaServer进程也是在init.rc中启动的，它不是一个daemon进程，这点容易搞混。他也是和systemserver进程类似的系统服务进程，提供应用进程的RPC调用的真正服务代码所运行的位置。其服务都是和媒体录播放有关，主要有三个服务：

- **AudioFlinger****
声音的录播放服务，包括混音等
- **MediaPlayerService**
提供媒体播放服务，opencore是这块的核心模块，对java端的接口在mediaplayer.java
- **CameraService**
提供camera的录制、preview等功能的服务
- **AudioPolicyService**
主要功能有检查输入输出设备的连接状态及系统的音频策略的切换等。
