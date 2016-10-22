---
layout: post
title: android开发错误笔记
category: Android
tags: android
keywords:
description:
---

1. `build-tools/22.0.1/aapt finished with non-zero exit value 127`  
使用 `./gradlew assembleDebug --info` 命令找到具体出错原因  
`/opt/local/android-sdk/android-sdk-linux/build-tools/22.0.1/aapt: error while loading shared libraries: libstdc++.so.6: cannot open shared object file: No such file or directory`  
原因是: 64位的Ubuntu系统, 应该是缺少 lib32stdc++6这个包，用 `apt-get install lib32stdc++6`安装一下就可以

2. `chromium: [INFO:CONSOLE(107)] "TypeError: Cannot read property 'getItem' of null`  
This seems to allow the browser to store a DOM model of the page elements, so that Javascript can perform operations on it.  
解决方法：
```
WebSettings settings = webView.getSettings();
settings.setDomStorageEnabled(true);
```

3. `Installation failed with message INSTALL_FAILED_CONFLICTING_PROVIDER`
The authority, as listed in android:authorities must be unique. Quoting the documentation for this attribute.  
解决方法：
```
To avoid conflicts, authority names should use a Java-style naming convention (such as com.example.provider.cartoonprovider). Typically, it's the name of the ContentProvider subclass that implements the provider
```

4. `java.lang.UnsatisfiedLinkError: dlopen failed: cannot locate symbol "tcgetattr" referenced by "libecgJni.so"...`  
NDK开发时，app运行崩溃。
```
E/AndroidRuntime(15368): FATAL EXCEPTION: main
E/AndroidRuntime(15368): Process: com.acmed.dim, PID: 15368
E/AndroidRuntime(15368): java.lang.UnsatisfiedLinkError: dlopen failed: cannot locate symbol "tcgetattr" referenced by "libecgJni.so"...
E/AndroidRuntime(15368):        at java.lang.Runtime.loadLibrary(Runtime.java:364)
E/AndroidRuntime(15368):        at java.lang.System.loadLibrary(System.java:555)
E/AndroidRuntime(15368):        at com.acmed.dim.ui.fragment.ecg.EcgJni.<clinit>(EcgJni.java:831)
E/AndroidRuntime(15368):        at com.acmed.dim.ui.EcgCollectionActivity.startService(EcgCollectionActivity.java:830)
E/AndroidRuntime(15368):        at com.acmed.dim.ui.EcgCollectionActivity.deviceReady(EcgCollectionActivity.java:1193)
E/AndroidRuntime(15368):        at com.acmed.dim.ui.EcgCollectionActivity.access$1500(EcgCollectionActivity.java:68)
E/AndroidRuntime(15368):        at com.acmed.dim.ui.EcgCollectionActivity$4.onReceive(EcgCollectionActivity.java:1224)
E/AndroidRuntime(15368):        at android.app.LoadedApk$ReceiverDispatcher$Args.run(LoadedApk.java:782)
E/AndroidRuntime(15368):        at android.os.Handler.handleCallback(Handler.java:733)
E/AndroidRuntime(15368):        at android.os.Handler.dispatchMessage(Handler.java:95)
E/AndroidRuntime(15368):        at android.os.Looper.loop(Looper.java:136)
E/AndroidRuntime(15368):        at android.app.ActivityThread.main(ActivityThread.java:5342)
E/AndroidRuntime(15368):        at java.lang.reflect.Method.invokeNative(NativeMethod)
E/AndroidRuntime(15368):        at java.lang.reflect.Method.invoke(Method.java:515)
E/AndroidRuntime(15368):        at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:871)
E/AndroidRuntime(15368):        at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:687)
E/AndroidRuntime(15368):        at dalvik.system.NativeStart.main(Native Method)
```
解决方法：
```
You can see the diff of termios.h file between API 19 and 21+.
so, I copy the termios.h from D:\Android\Sdk\ndk-bundle\platforms\android-19\arch-arm\usr\include to jni folder, and then it works.
```

5.  `com.android.dex.DexException: Multiple dex files define Lcom/XunLiu/jni/NativeInitializer`  
这个问题出现在一个模块与依赖的模块都导入了同一个jar包。  
This happens when a Module has a dependency on both another module and that same module's jar.  
解决方法：
```
删除相同的jar包
```
