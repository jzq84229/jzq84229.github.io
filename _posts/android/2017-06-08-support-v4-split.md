---
layout: post
title: Android最新Support V4包拆分
category: Android
tags: support-v4
keywords: support-v4
description:
---

Google最近将V4 Support Library拆分成5个子modules，每个Module可以被单独引用。
![Alt text](http://7xn4nm.com1.z0.glb.clouddn.com/support-v4-all.png)


### 1. 子Module介绍
- #### support-compat  
  封装兼容性framework API。如Context.getDrawable() 和 View.performAccessibilityAction()。
- #### support-core-utils  
  提供一些核心的工具，如 AsyncTaskLoader 和 PermissionChecke。
- #### support-core-ui  
  提供一系列核心的UI，如 ViewPager, NestedScrollView, 和 ExploreByTouchHelper。
- #### support-media-compat
  android.media兼容库，包括 MediaBrowser 和 MediaSession。
- #### support-fragment
  fragment兼容库。这个库依赖support-compat, support-core-utils, support-core-ui, and support-media-compat.



### 2. 子Module间依赖关系
 ![Alt text](http://7xn4nm.com1.z0.glb.clouddn.com/support-v4-dependency.png)

PS：其中 support-annotations 为一些注解的声明库，如我们比较常用的 RequiresApi、UiThread、NonNull。

参考资料
---
[Support Library Revision Archive](https://developer.android.com/topic/libraries/support-library/rev-archive.html#24-2-0-v4-refactor)  
[Android 最新 Support V4 包大拆分有用吗](http://b.codekk.com/detail/Trinea/Android%20%E6%9C%80%E6%96%B0%20Support%20V4%20%E5%8C%85%E5%A4%A7%E6%8B%86%E5%88%86%E6%9C%89%E7%94%A8%E5%90%97%EF%BC%9F)
