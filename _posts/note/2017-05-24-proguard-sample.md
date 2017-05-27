---
layout: post
title: 常用第三方组件代码混淆
category: 笔记
tags: 混淆
keywords:
description:
---
## 不能混淆的代码
顾名思义，不能混淆代码如果被混淆了，就会出现错误。

1. 需要反射的代码
2. 系统接口
3. Jni接口
4. 需要序列号和反序列化的代码（即实现Serializable接口的JavaBean）
5. 与服务端进行元数据交互的JavaBean（JSON、XML中对应的类）

## 常见问题

1. 打包过程中，提示 `Warning: can't find superclass or interface/ Warning: can't find referenced class`等警告信息！
解决方法：  
  1. 确保你的代码是否使用这个类，如果使用了，查看对应的第三方包有没有加上去，并且是否拉下来存在项目中  
  2. 如果该类存在，工程中确实使用了这个类，就在proguard中加上`-keep class com.xx.yy.** { *;}`，让当前类不混淆。  
  3. 确保报错的类没有在你的项目中使用到,可以使用"-dontwarn 类名正则表达式"屏蔽警告。
2. 程序中使用泛型导致运行错误！  
使用的泛型需要在混淆配置文件加了一个过滤泛型的语句，如下。  
`-keepattributes Signature`

## 第三方组件代码混淆

### support-v4
#@link https://stackoverflow.com/questions/18978706/obfuscate-android-support-v7-widget-gridlayout-issue
```
-dontwarn android.support.v4.**
-keep class android.support.v4.app.** { *; }
-keep interface android.support.v4.app.** { *; }
-keep class android.support.v4.** { *; }
```

### support-v7
```
-dontwarn android.support.v7.**
-keep class android.support.v7.internal.** { *; }
-keep interface android.support.v7.internal.** { *; }
-keep class android.support.v7.** { *; }
```

### support design
#@link http://stackoverflow.com/a/31028536
```
-dontwarn android.support.design.**
-keep class android.support.design.** { *; }
-keep interface android.support.design.** { *; }
-keep public class android.support.design.R$* { *; }
```

### error : Note: the configuration refers to the unknown class 'com.google.vending.licensing.ILicensingService'
#solution : @link http://stackoverflow.com/a/14463528
```
-dontnote com.google.vending.licensing.ILicensingService
-dontnote **ILicensingService
```

### RxJava RxAndroid
```
-dontwarn sun.misc.**
-keepclassmembers class rx.internal.util.unsafe.*ArrayQueue*Field* {
    long producerIndex;
    long consumerIndex;
}
-keepclassmembers class rx.internal.util.unsafe.BaseLinkedQueueProducerNodeRef {
    rx.internal.util.atomic.LinkedQueueNode producerNode;
}
-keepclassmembers class rx.internal.util.unsafe.BaseLinkedQueueConsumerNodeRef {
    rx.internal.util.atomic.LinkedQueueNode consumerNode;
}
```

### Retrofit
```
-dontwarn retrofit.**
-keep class retrofit.** { *; }
-keepattributes Signature
-keepattributes Exceptions
```

### Gson
```
配置1
-keepattributes Signature
-keepattributes *Annotation*
-keep class sun.misc.Unsafe { *; }
-keep class com.google.gson.stream.** { *; }
# Application classes that will be serialized/deserialized over Gson
-keep class com.sunloto.shandong.bean.** { *; }

配置2
-dontwarn com.google.gson.**
-keep class com.google.gson.** {*;}
-keep class com.google.**{*;}
-keep class sun.misc.Unsafe { *; }
-keep class com.google.gson.stream.** { *; }
-keep class com.google.gson.examples.android.model.** { *; }
```

### FastJson
```
配置1
-dontwarn com.alibaba.fastjson.**
-keep class com.alibaba.fastjson.** {*;}
-keepattributes Signature
-keepattributes *Annotation*

配置2
-keep class  com.alibaba.fastjson.** {*;}
-dontwarn com.alibaba.fastjson.**
-keepclassmembers class * {
public <methods>;
}
```

### OkHttp
```
-keepattributes Signature
-keepattributes *Annotation*
-keep class com.squareup.okhttp.** { *; }
-keep interface com.squareup.okhttp.** { *; }
-dontwarn com.squareup.okhttp.**
```

### OkHttp3
```
-dontwarn com.squareup.okhttp3.**
-keep class com.squareup.okhttp3.** { *;}
-dontwarn okio.**
```

### Okio
```
-keep class sun.misc.Unsafe { *; }
-dontwarn java.nio.file.*
-dontwarn org.codehaus.mojo.animal_sniffer.IgnoreJRERequirement
-dontwarn okio.**
```

### android-async-http
```
-dontwarn com.loopj.android.http.**
-keep class com.loopj.android.http.** { *; }
```

### httpcore
```
-libraryjars libs/httpcore-4.3.2.jar
-dontwarn  org.apache.http.**
-keep class org.apache.http.** { *; }
```

### Universal-Image-Loader-v1.9.5
```
-libraryjars libs/universal-image-loader-1.9.5-SNAPSHOT-with-sources.jar
-dontwarn com.nostra13.universalimageloader.**
-keep class com.nostra13.universalimageloader.** { *; }
```

### Picasso
```
-dontwarn com.squareup.okhttp.**
```

### PhotoView
```
-dontwarn uk.co.senab.photoview.**
-keep class uk.co.senab.photoview.** {*;}
```

### 七牛
```
-keep class com.qiniu.**{*;}
-keep class com.qiniu.**{public <init>();}
-ignorewarnings
```

### yuntongxun
```
#注：v.x.x.x根据实际版本号修改，例如v5.0.0.1b
#-libraryjars libs/Yuntx_IMLib_vx.x.x.jar(如果是Android Studio 此行忽略)
-keep class com.yuntongxun.ecsdk.** {*;}
```

### pingyin4j
```
-dontwarn net.soureceforge.pinyin4j.**
-dontwarn demo.**
-libraryjars libs/pinyin4j-2.5.0.jar
-keep class net.sourceforge.pinyin4j.** {*;}
-keep class demo.** { *;}
```
