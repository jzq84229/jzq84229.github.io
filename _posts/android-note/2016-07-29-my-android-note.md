---
layout: post
title: Android个人笔记
category: Android
tags:
keywords:
description:
---

### 1. 计时器:Chonometer
Chronometer是TextView的子类，也是一个Android组件。这个组件可以用1秒的时间间隔进行计时，并显示出计时结果。  
[http://www.cnblogs.com/over140/archive/2010/11/22/1883736.html](http://www.cnblogs.com/over140/archive/2010/11/22/1883736.html)  
[https://developer.android.com/reference/android/widget/Chronometer.html](https://developer.android.com/reference/android/widget/Chronometer.html)

### 2. onCreate()内参数设置设置
```
requestWindowFeature(Window.FEATURE_NO_TITLE);  //设置无标题
getWindow().setFlags(WindowManager.LayoutParams.FLAG_FULLSCREEN,WindowManager.LayoutParams.FLAG_FULLSCREEN);  //设置全屏
getWindow().setType(WindowManager.LayoutParams.TYPE_KEYGUARD);  //网上都说是屏蔽HOME键，但是操作起来发现没有效果，我再查查是什么作用。。。
getWindow().setFormat(PixelFormat.RGBA_8888); //防止图片失真
```

### 3. getWindow().setFormat(PixelFormat.XXX); 设置窗口像素格式
像素格式：在创建一个图形操作表之前，首先必须设置像素格式。像素格式含有设备绘图界面的属性，这些属性包括绘图界面是用RGBA模式还是颜色表模式，像素缓存是用单缓存还是双缓存，以及颜色位数、深度缓存和模板缓存所用的位数，还有其它一些属性信息。  
像素格式说明可参考:   [http://www.voidcn.com/blog/jiangdf/article/p-3131656.html](http://www.voidcn.com/blog/jiangdf/article/p-3131656.html)   
Android PixelFormat(像素格式)可参考:  
[https://developer.android.com/reference/android/graphics/PixelFormat.html](https://developer.android.com/reference/android/graphics/PixelFormat.html)  
[http://www.cnblogs.com/tgyf/p/4681607.html](http://www.cnblogs.com/tgyf/p/4681607.html)

### 4. ViewStub
ViewStub是一个轻量级的View，它一个看不见的，不占布局位置，占用资源非常小的控件。可以为ViewStub指定一个布局，在Inflate布局的时候，只有ViewStub会被初始化，然后当ViewStub被设置为可见的时候，或是调用了ViewStub.inflate()的时候，ViewStub所向的布局就会被Inflate和实例化，然后ViewStub的布局属性都会传给它所指向的布局。这样，就可以使用ViewStub来方便的在运行时，要还是不要显示某个布局。  

但ViewStub也不是万能的，下面总结下ViewStub能做的事儿和什么时候该用ViewStub，什么时候该用可见性的控制。  
首先来说说ViewStub的一些特点：
1. ViewStub只能Inflate一次，之后ViewStub对象会被置为空。按句话说，某个被ViewStub指定的布局被Inflate后，就不会够再通过ViewStub来控制它了。
2. ViewStub只能用来Inflate一个布局文件，而不是某个具体的View，当然也可以把View写在某个布局文件中。

基于以上的特点，那么可以考虑使用ViewStub的情况有：
1. 在程序的运行期间，某个布局在Inflate后，就不会有变化，除非重新启动。  
因为ViewStub只能Inflate一次，之后会被置空，所以无法指望后面接着使用ViewStub来控制布局。所以当需要在运行时不止一次的显示和隐藏某个布局，那么ViewStub是做不到的。这时就只能使用View的可见性来控制了。
2. 想要控制显示与隐藏的是一个布局文件，而非某个View。  
因为设置给ViewStub的只能是某个布局文件的Id，所以无法让它来控制某个View。  

所以，如果想要控制某个View(如Button或TextView)的显示与隐藏，或者想要在运行时不断的显示与隐藏某个布局或View，只能使用View的可见性来控制。

实例可见：  
[http://blog.csdn.net/jason0539/article/details/26132267](http://blog.csdn.net/jason0539/article/details/26132267)  
[http://blog.csdn.net/hitlion2008/article/details/6737537](http://blog.csdn.net/hitlion2008/article/details/6737537)  

### 5. Interpolator 动画插值器
可参考以下文章：
[http://androidigging.blog.51cto.com/2753843/1427128](http://androidigging.blog.51cto.com/2753843/1427128)

### 6. 设置ScrollView中ScrollBar的Style
在xml中配置`android:scrollbarStyle="insideInset"`属性。
有4个属性可选：
- outsideInset:
- outsideOverlay:
- insideInset:
- inside

### 7.Android抽象布局——include、merge 、ViewStub
Android抽象布局——include、merge 、ViewStub: [http://blog.csdn.net/xyz_lmn/article/details/14524567](http://blog.csdn.net/xyz_lmn/article/details/14524567)  
 Android布局优化之ViewStub、include、merge使用与源码分析: [http://blog.csdn.net/bboyfeiyu/article/details/45869393](http://blog.csdn.net/bboyfeiyu/article/details/45869393)  
Android布局优化之include、merge、ViewStub的使用
:[http://www.sunnyang.com/418.html?utm_source=tuicool&utm_medium=referral](http://www.sunnyang.com/418.html?utm_source=tuicool&utm_medium=referral)  
android中include、merge、ViewStub使用与源码分析: [http://www.jianshu.com/p/5105cc71a3e1](http://www.jianshu.com/p/5105cc71a3e1)

### 8.ToolBar介绍
Toolbar: [https://developer.android.com/reference/android/widget/Toolbar.html](https://developer.android.com/reference/android/widget/Toolbar.html)  
Android开发：最详细的 Toolbar 开发实践总结: [http://www.jianshu.com/p/79604c3ddcae](http://www.jianshu.com/p/79604c3ddcae)  
Toolbar的介绍和使用: [http://blog.csdn.net/wangjiang_qianmo/article/details/50673176](http://blog.csdn.net/wangjiang_qianmo/article/details/50673176)  
Android ToolBar 使用完全解析: [http://www.jianshu.com/p/ae0013a4f71a](http://www.jianshu.com/p/ae0013a4f71a)  
android：ToolBar详解（手把手教程）: [http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2014/1118/2006.html](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2014/1118/2006.html)  
Android ToolBar 的简单封装: [http://blog.csdn.net/jxxfzgy/article/details/46476903](http://blog.csdn.net/jxxfzgy/article/details/46476903)  

### 9.保持屏幕常亮
[http://kaywu.github.io/2016/04/17/Android-Screen-On/](http://kaywu.github.io/2016/04/17/Android-Screen-On/)  
[http://blog.csdn.net/panda1234lee/article/details/8731636](http://blog.csdn.net/panda1234lee/article/details/8731636)  

### 10.surfaceView设置背景透明
创建时：
```
setZOrderOnTop(true);
getHolder().setFormat(PixelFormat.TRANSLUCENT);
```

绘制时：
```
Canvas.drawColor(Color.TRANSPARENT,Mode.CLEAR);
```

### 11.平板Pad使用TabLayout时无法全屏显示
这是由于TabLayout的默认样式Widget.Design.TabLayout在不同设备继承自不同样式。 (normal, landscape and sw600dp)
我们要看的是平板样式tablets (sw600dp)
```
<style name="Widget.Design.TabLayout" parent="Base.Widget.Design.TabLayout">
    <item name="tabGravity">center</item>
    <item name="tabMode">fixed</item>
</style>
```
这里的属性`tabGravity`我们使用的`fill`  
我们再看一下这个样式继承的`Base.Widget.Design.TabLayout`
```
<style name="Base.Widget.Design.TabLayout" parent="android:Widget">
    <item name="tabMaxWidth">@dimen/tab_max_width</item>
    <item name="tabIndicatorColor">?attr/colorAccent</item>
    <item name="tabIndicatorHeight">2dp</item>
    <item name="tabPaddingStart">12dp</item>
    <item name="tabPaddingEnd">12dp</item>
    <item name="tabBackground">?attr/selectableItemBackground</item>
    <item name="tabTextAppearance">@style/TextAppearance.Design.Tab</item>
    <item name="tabSelectedTextColor">?android:textColorPrimary</item>
</style>
```
所以从这个样式我们知道应该重写`tabMaxWidth`属性。我这设置为0dp，这样就没有宽度限制了。  
详细信息可见：[http://stackoverflow.com/questions/30843775/tab-not-taking-full-width-on-tablet-device-using-android-support-design-widget](http://stackoverflow.com/questions/30843775/tab-not-taking-full-width-on-tablet-device-using-android-support-design-widget)


### 12.EditText如何不自动获取焦点
android页面中如有EditText，则进入页面后EditText会自动获取焦点，如何取消这种默认行为呢？
只需在EditText的父控件上设置
```
android:focusable="true"  
android:focusableInTouchMode="true"
 ```
 这样就拦截了EditText默认获取焦点的行为。

### 13.Sources for 'Android API 25 Platform' not found
进入Android SDK设置界面  
Windows: File -> Settings (ctrl+alt+s) -> Appearance & Behavior -> System Settings -> Android SDK.  
Mac: Android Studio -> Preferences (cmd + ,) -> Appearance & Behavior -> System Settings -> Android SDK.  
点击Android SDK路径右侧的`Edit`按钮，然后根据设置向导点击`Next`按钮。即可修复这个问题。

[http://stackoverflow.com/questions/36814755/sources-for-android-api-23-platform-not-found-android-studio-2-0](http://stackoverflow.com/questions/36814755/sources-for-android-api-23-platform-not-found-android-studio-2-0)
