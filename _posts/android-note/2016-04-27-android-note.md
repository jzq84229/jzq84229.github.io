---
layout: post
title: android笔记
category: Android
tags: Android
keywords:
description:
---

## Intent and Intent Filters
Intent 分为两种类型：

显式 Intent ：按名称（完全限定类名）指定要启动的组件。通常，您会在自己的应用中使用显式 Intent 来启动组件，这是因为您知道要启动的 Activity 或服务的类名。例如，启动新 Activity 以响应用户操作，或者启动服务以在后台下载文件。  
隐式 Intent ：不会指定特定的组件，而是声明要执行的常规操作，从而允许其他应用中的组件来处理它。例如，如需在地图上向用户显示位置，则可以使用隐式 Intent，请求另一具有此功能的应用在地图上显示指定的位置。

> 警告：为了确保应用的安全性，启动 Service 时，请始终使用显式 Intent，且不要为服务声明 Intent 过滤器。使用隐式 Intent 启动服务存在安全隐患，因为您无法确定哪些服务将响应 Intent，且用户无法看到哪些服务已启动。从 Android 5.0（API 级别 21）开始，如果使用隐式 Intent 调用 bindService()，系统会抛出异常。

> 警告：用户可能没有任何应用处理您发送到 startActivity() 的隐式 Intent。如果出现这种情况，则调用将会失败，且应用会崩溃。要验证 Activity 是否会接收 Intent，请对 Intent 对象调用 resolveActivity()。如果结果为非空，则至少有一个应用能够处理该 Intent，且可以安全调用 startActivity()。如果结果为空，则不应使用该 Intent。如有可能，您应禁用发出该 Intent 的功能。

    // Create the text message with a string
    Intent sendIntent = new Intent();
    sendIntent.setAction(Intent.ACTION_SEND);
    sendIntent.putExtra(Intent.EXTRA_TEXT, textMessage);
    sendIntent.setType("text/plain");
    
    // Verify that the intent will resolve to an activity
    if (sendIntent.resolveActivity(getPackageManager()) != null) {
        startActivity(sendIntent);
    }

### 强制使用应用选择器
如果多个应用可以响应 Intent，且用户可能希望每次使用不同的应用，则应采用显式方式显示选择器对话框。选择器对话框要求用户选择每次操作要使用的应用（用户无法为该操作选择默认应用）。 例如，当应用使用 ACTION_SEND 操作执行“共享”时，用户根据目前的状况可能需要使用另一不同的应用，因此应当始终使用选择器对话框。  
要显示选择器，请使用 createChooser() 创建 Intent，并将其传递给 startActivity()。例如：

    Intent sendIntent = new Intent(Intent.ACTION_SEND);
    ...
    
    // Always use string resources for UI text.
    // This says something like "Share this photo with"
    String title = getResources().getString(R.string.chooser_title);
    // Create intent to show the chooser dialog
    Intent chooser = Intent.createChooser(sendIntent, title);
    
    // Verify the original intent will resolve to at least one activity
    if (sendIntent.resolveActivity(getPackageManager()) != null) {
        startActivity(chooser);
    }

这将显示一个对话框，其中包含响应传递给 createChooser() 方法的 Intent 的应用列表，并使用提供的文本作为对话框标题。

### 接收隐式 Intent

> 注意：为了接收隐式 Intent，必须将 CATEGORY_DEFAULT 类别包括在 Intent 过滤器中。方法 startActivity() 和 startActivityForResult() 将按照已申明 CATEGORY_DEFAULT 类别的方式处理所有 Intent。 如果未在 Intent 过滤器中声明此类别，则隐式 Intent 不会解析为您的 Activity。

> 警告：为了避免无意中运行不同应用的 Service，请始终使用显式 Intent 启动您自己的服务，且不必为该服务声明 Intent 过滤器。

## Activity

### 实现生命周期回调
 Activity 生命周期。
 ![Activity 生命周期](http://winy.eicp.net/activity_lifecycle.png)

表 1. Activity 生命周期回调方法汇总表。

|方法	|描述	|是否能事后终止？	|后接|
|:---|:---|:---|:---|
|**onCreate()**|首次创建 Activity 时调用。 您应该在此方法中执行所有正常的静态设置— 创建视图、将数据绑定到列表等等。系统向此方法传递一个 Bundle 对象，其中包含 Activity 的上一状态，不过前提是捕获了该状态（请参阅后文的保存 Activity 状态）。始终后接 onStart()。|否	|onStart()|
|**onRestart()**|在 Activity 已停止并即将再次启动前调用。始终后接 onStart()|否	|onStart()|
|**onStart()**|在 Activity 即将对用户可见之前调用。如果 Activity 转入前台，则后接 onResume()，如果 Activity 转入隐藏状态，则后接 onStop()。|否|onResume()或onStop()|
|**onResume()**|在 Activity 即将开始与用户进行交互之前调用。 此时，Activity 处于 Activity 堆栈的顶层，并具有用户输入焦点。始终后接 onPause()。|否	|onPause()|
|**onPause()**|当系统即将开始继续另一个 Activity 时调用。 此方法通常用于确认对持久性数据的未保存更改、停止动画以及其他可能消耗 CPU 的内容，诸如此类。 它应该非常迅速地执行所需操作，因为它返回后，下一个 Activity 才能继续执行。如果 Activity 返回前台，则后接 onResume()，如果 Activity 转入对用户不可见状态，则后接 onStop()。|**是**|onResume() 或onStop()|
|**onStop()**|Activity 对用户不再可见时调用。如果 Activity 被销毁，或另一个 Activity（一个现有 Activity 或新 Activity）继续执行并将其覆盖，就可能发生这种情况。如果 Activity 恢复与用户的交互，则后接 onRestart()，如果 Activity 被销毁，则后接 onDestroy()。|**是**|onRestart() 或 onDestroy()|
|**onDestroy()**|在 Activity 被销毁前调用。这是 Activity 将收到的最后调用。 当 Activity 结束（有人调用 Activity 上的 finish()），或系统为节省空间而暂时销毁该 Activity 实例时，可能会调用它。 您可以通过 isFinishing() 方法区分这两种情形。|**是**|无|

### 保存 Activity 状态

## Fragment

![fragment 生命周期](http://winy.eicp.net/fragment_lifecycle.png)


## Loader加载器

[Loader](https://developer.android.com/intl/zh-cn/guide/components/loaders.html)

## Tasks and Back Stack任务和返回栈


## Services 服务
[https://developer.android.com/intl/zh-cn/guide/components/services.html](https://developer.android.com/intl/zh-cn/guide/components/services.html)


## Handling Runtime Changes处理运行时变更

[Handling Runtime Changes](https://developer.android.com/intl/zh-cn/guide/topics/resources/runtime-changes.html)
setRetainInstance(boolean)

## menu

## dialog
