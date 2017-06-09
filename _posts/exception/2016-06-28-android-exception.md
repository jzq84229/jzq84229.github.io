---
layout: post
title: android开发错误笔记
category: Exception
tags: android
keywords:
description:
---

#### 1. `build-tools/22.0.1/aapt finished with non-zero exit value 127`  
使用 `./gradlew assembleDebug --info` 命令找到具体出错原因  
`/opt/local/android-sdk/android-sdk-linux/build-tools/22.0.1/aapt: error while loading shared libraries: libstdc++.so.6: cannot open shared object file: No such file or directory`  
原因是: 64位的Ubuntu系统, 应该是缺少 lib32stdc++6这个包，用 `apt-get install lib32stdc++6`安装一下就可以

#### 2. `chromium: [INFO:CONSOLE(107)] "TypeError: Cannot read property 'getItem' of null`  
This seems to allow the browser to store a DOM model of the page elements, so that Javascript can perform operations on it.  
解决方法：
```
WebSettings settings = webView.getSettings();
settings.setDomStorageEnabled(true);
```

#### 3. `Installation failed with message INSTALL_FAILED_CONFLICTING_PROVIDER`
The authority, as listed in android:authorities must be unique. Quoting the documentation for this attribute.  
解决方法：
```
To avoid conflicts, authority names should use a Java-style naming convention (such as com.example.provider.cartoonprovider). Typically, it's the name of the ContentProvider subclass that implements the provider
```

#### 4. `java.lang.UnsatisfiedLinkError: dlopen failed: cannot locate symbol "tcgetattr" referenced by "libecgJni.so"...`  
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

#### 5.  `com.android.dex.DexException: Multiple dex files define Lcom/XunLiu/jni/NativeInitializer`  
这个问题出现在一个模块与依赖的模块都导入了同一个jar包。  
This happens when a Module has a dependency on both another module and that same module's jar.  
解决方法：
```
删除相同的jar包
```

#### 6. `java.lang.IndexOutOfBoundsException: Invalid item position 0(0). Item count:0`
RecyclerView自定义LayoutManage时，在onmeasure()中调用
`View view = recycler.getViewForPosition(position);`报以下错误。  
```
03-24 09:40:20.887 25006-25006/com.acmed.multiparams.monitor E/AndroidRuntime: FATAL EXCEPTION: main
 Process: com.acmed.multiparams.monitor, PID: 25006
 java.lang.IndexOutOfBoundsException: Invalid item position 0(0). Item count:0
     at android.support.v7.widget.RecyclerView$Recycler.tryGetViewHolderForPositionByDeadline(RecyclerView.java:5466)
     at android.support.v7.widget.RecyclerView$Recycler.getViewForPosition(RecyclerView.java:5440)
     at android.support.v7.widget.RecyclerView$Recycler.getViewForPosition(RecyclerView.java:5436)
     at com.acmed.multiparams.monitor.common.widget.AutoGridLayoutManager.onMeasure(AutoGridLayoutManager.java:39)
     at android.support.v7.widget.RecyclerView.onMeasure(RecyclerView.java:3014)
     at com.acmed.multiparams.monitor.common.widget.PageRecyclerView.onMeasure(PageRecyclerView.java:99)
     at android.view.View.measure(View.java:17682)
     at android.widget.LinearLayout.measureVertical(LinearLayout.java:875)
     at android.widget.LinearLayout.onMeasure(LinearLayout.java:613)
     at android.view.View.measure(View.java:17682)
     at android.view.ViewGroup.measureChildWithMargins(ViewGroup.java:5535)
     at android.widget.FrameLayout.onMeasure(FrameLayout.java:438)
     at android.support.v7.widget.ContentFrameLayout.onMeasure(ContentFrameLayout.java:139)
     at android.view.View.measure(View.java:17682)
     at android.view.ViewGroup.measureChildWithMargins(ViewGroup.java:5535)
     at android.widget.LinearLayout.measureChildBeforeLayout(LinearLayout.java:1436)
     at android.widget.LinearLayout.measureVertical(LinearLayout.java:722)
     at android.widget.LinearLayout.onMeasure(LinearLayout.java:613)
     at android.view.View.measure(View.java:17682)
     at android.view.ViewGroup.measureChildWithMargins(ViewGroup.java:5535)
     at android.widget.FrameLayout.onMeasure(FrameLayout.java:438)
     at android.view.View.measure(View.java:17682)
     at android.view.ViewGroup.measureChildWithMargins(ViewGroup.java:5535)
     at android.widget.LinearLayout.measureChildBeforeLayout(LinearLayout.java:1436)
     at android.widget.LinearLayout.measureVertical(LinearLayout.java:722)
     at android.widget.LinearLayout.onMeasure(LinearLayout.java:613)
     at android.view.View.measure(View.java:17682)
     at android.view.ViewGroup.measureChildWithMargins(ViewGroup.java:5535)
     at android.widget.FrameLayout.onMeasure(FrameLayout.java:438)
     at com.android.internal.policy.impl.PhoneWindow$DecorView.onMeasure(PhoneWindow.java:2789)
     at android.view.View.measure(View.java:17682)
     at android.view.ViewRootImpl.performMeasure(ViewRootImpl.java:2131)
     at android.view.ViewRootImpl.measureHierarchy(ViewRootImpl.java:1273)
     at android.view.ViewRootImpl.performTraversals(ViewRootImpl.java:1491)
     at android.view.ViewRootImpl.doTraversal(ViewRootImpl.java:1161)
     at android.view.ViewRootImpl$TraversalRunnable.run(ViewRootImpl.java:6198)
     at android.view.Choreographer$CallbackRecord.run(Choreographer.java:767)
     at android.view.Choreographer.doCallbacks(Choreographer.java:580)
     at android.view.Choreographer.doFrame(Choreographer.java:550)
     at android.view.Choreographer$FrameDisplayEventReceiver.run(Choreographer.java:753)
     at android.os.Handler.handleCallback(Handler.java:739)
     at android.os.Handler.dispatchMessage(Handler.java:95)
     at android.os.Looper.loop(Looper.java:135)
     at android.app.ActivityThread.main(ActivityThread.java:5280)
     at java.lang.reflect.Method.invoke(Native Method)
     at java.lang.reflect.Method.invoke(Method.java:372)
     at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:963)
     at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:758)
```
解决方法：  
在LayoutManager构造函数中调用`setAutoMeasureEnabled(false);`

#### 7. `ECSDK couldn't start: dlopen failed: /data/app/com.acmedcare.im.imapp-1/lib/arm/libserphone.so: has text relocations`  
手机：小米4   
系统：MIUI8.2稳定版 Android 6.0.1 MMB29M  
应用使用容联云SDK的so文件。  
gradle targetSdkVersion大于22，运行时报错。  

解决方案：  
gradle targetSdkVersion 改为22。打包后正常运行。
原因待查。

#### 8. `Failure [INSTALL_FAILED_UPDATE_INCOMPATIBLE]`
adb安装APK时报错：`Failure [INSTALL_FAILED_UPDATE_INCOMPATIBLE]`  
在已安装Release版APK后，覆盖安装Debug版APK时，在未完全卸载的一个无效状态被卡住le。  
解决方法：  
打开命令行界面，运行：`adb uninstall my.package.id`
这样便可以完全卸载应用。  
[http://stackoverflow.com/questions/26794862/failure-install-failed-update-incompatible-even-if-app-appears-to-not-be-insta](http://stackoverflow.com/questions/26794862/failure-install-failed-update-incompatible-even-if-app-appears-to-not-be-insta)

#### 9. `Error:(95) undefined reference to '__android_log_print'`
NDK运行时报错：

解决方法，用eclispe编译：
```
在Android.mk中添加
LOCAL_LDLIBS := -llog
```
如果使用Android Studio和gradle，会忽略Android.md文件，则需要在build.gradle文件中添加:
```
android {
    defaultConfig {
        ndk {
            moduleName "your_module_name"
            ldLibs "log"
        }
    }
}
```
[http://stackoverflow.com/questions/4455941/undefined-reference-to-android-log-print](http://stackoverflow.com/questions/4455941/undefined-reference-to-android-log-print)
