---
layout: post
title: EventBus的onEvent被多次执行
category: Android
tags: Android
keywords:
description:
---

最近项目开发一个评论功能A页面，另有B页面，C页面。A点击按钮后startActivityForResult()进入B页面，B点击按钮进入C页面同时finish B和A页面。C点击按钮后进入A同时finish C。  
在A页面发表评论时发现在某些情况下会多有条相同内容的评论，查看后台数据库发现确实创建了多条相同内容的评论。  
初步估计可能为消息系统重复发送评论请求。  
查看服务器日志，确实在同一时间执行了多次添加评论的方法。  
查看nginx日志，发现在同一时间有多条同一内容的添加评论请求，确定bug为客户端多次发送添加评论请求。  

可发送评论按钮已经添加防重复点击判断，怎么会在同一时间（多次请求间隔仅几毫秒）发送多个添加评论请求呢？  
在发送按钮点击事件添加log，请求发送前添加log，观察log，点击事件确实只触发一次，但却发送了多次添加评论请求。  
发送评论请求使用的是EventBus的异步事件，到此可以确定问题在EventBus的post订阅上，网上查找资料，基本确定是eventBus没有正确unregister。

查看代码，在onCreateView方法中register eventBus，在onDestroyView中unregister eventBus，确定没有问题。又一次陷入头疼中。
再次查看log发现A页面的onDestroyView方法没有被调用。查看B页面代码，B点击进入C时的代码：

    startActivity(new Intent(this, C.class));
    Intent intent = new Intent();
    setResult(RESULT_OK, intent);
    finish();

而A页面onActivityResult函数没有执行。仔细一看B的代码startActivity(new Intent(this, C.class))页面跳转到了C页面，A的onActivityResult没有执行。 查看onActivityResult源码为一个空函数。查看api

>  protected void onActivityResult (int  requestCode, int resultCode, Intent  data)  
>  Since:API Level 1 Called when an activity you launched exits, giving you the requestCode you started it with, the resultCode it returned, and any additional data from it. The resultCode will be RESULT_CANCELED if the activity explicitly returned that, didn't return any result, or crashed during its operation.  
>  You will receive this call immediately before onResume() when your activity is re-starting.  
>  This method is never invoked if your activity sets noHistory to true.

翻译:
>protected void onActivityResult (int  requestCode, int resultCode, Intent  data)

>当你调用完一个存在的activity之后，onActivityResult将会返回以下数据：你调用时发出的requestCode、被调用activity的结果标志resultCode（如RESULT_OK）和其他的额外数据。我们期望的都是得到RESULT_OK，表示调用成功，但是当被调用activity什么也没返回，或者调用过程中发生崩溃时，resultCode的值会为RESULT_CANCELED，重新回到调用activity时会马上执行onActivityResult方法，然后才是onResume()方法。  
如果你的activity设置noHistory为true，则这个方法永远不会被调用。  
而项目中页面跳转到了C页面，没有返回A页面，所以A的onActivityResult没有执行。  
将B中startActivity(new Intent(this, C.class));移到A的onActivityResult中，日志显示A的onDestroyView顺利执行，问题顺利解决。  

但是为什么EventBus，没有unRegister后eventBus的事件会多次执行呢？
查看EventBus的register源码:

	private synchronized void register(Object subscriber, boolean sticky, int priority) {
        List<SubscriberMethod> subscriberMethods = subscriberMethodFinder.findSubscriberMethods(subscriber.getClass());
        for (SubscriberMethod subscriberMethod : subscriberMethods) {
            subscribe(subscriber, subscriberMethod, sticky, priority);
        }
    }  

首先看register函数的三个参数  
subscriber：		就是要注册的订阅者  
sticky:			表示是否是粘性的，一般默认都是false，除非你调用registerSticky方法了  
priority：		表示事件的优先级，默认就行  

看第一句代码：

	List<SubscriberMethod> subscriberMethods = subscriberMethodFinder.findSubscriberMethods(subscriber.getClass());

这是从订阅者中查询所有订阅方法，即所有onEvent开头的方法。

	List<SubscriberMethod> findSubscriberMethods(Class<?> subscriberClass) {
        String key = subscriberClass.getName();
        List<SubscriberMethod> subscriberMethods;
        synchronized (methodCache) {
            subscriberMethods = methodCache.get(key);
        }
        if (subscriberMethods != null) {
            return subscriberMethods;
        }
        subscriberMethods = new ArrayList<SubscriberMethod>();
        Class<?> clazz = subscriberClass;
        HashSet<String> eventTypesFound = new HashSet<String>();
        StringBuilder methodKeyBuilder = new StringBuilder();
        while (clazz != null) {
            String name = clazz.getName();
            if (name.startsWith("java.") || name.startsWith("javax.") || name.startsWith("android.")) {
                // Skip system classes, this just degrades performance
                break;
            }

            // Starting with EventBus 2.2 we enforced methods to be public (might change with annotations again)
            Method[] methods = clazz.getDeclaredMethods();
            for (Method method : methods) {
                String methodName = method.getName();
                if (methodName.startsWith(ON_EVENT_METHOD_NAME)) {
                    int modifiers = method.getModifiers();
                    if ((modifiers & Modifier.PUBLIC) != 0 && (modifiers & MODIFIERS_IGNORE) == 0) {
                        Class<?>[] parameterTypes = method.getParameterTypes();
                        if (parameterTypes.length == 1) {
                            String modifierString = methodName.substring(ON_EVENT_METHOD_NAME.length());
                            ThreadMode threadMode;
                            if (modifierString.length() == 0) {
                                threadMode = ThreadMode.PostThread;
                            } else if (modifierString.equals("MainThread")) {
                                threadMode = ThreadMode.MainThread;
                            } else if (modifierString.equals("BackgroundThread")) {
                                threadMode = ThreadMode.BackgroundThread;
                            } else if (modifierString.equals("Async")) {
                                threadMode = ThreadMode.Async;
                            } else {
                                if (skipMethodVerificationForClasses.containsKey(clazz)) {
                                    continue;
                                } else {
                                    throw new EventBusException("Illegal onEvent method, check for typos: " + method);
                                }
                            }
                            Class<?> eventType = parameterTypes[0];
                            methodKeyBuilder.setLength(0);
                            methodKeyBuilder.append(methodName);
                            methodKeyBuilder.append('>').append(eventType.getName());
                            String methodKey = methodKeyBuilder.toString();
                            if (eventTypesFound.add(methodKey)) {
                                // Only add if not already found in a sub class
                                subscriberMethods.add(new SubscriberMethod(method, threadMode, eventType));
                            }
                        }
                    } else if (!skipMethodVerificationForClasses.containsKey(clazz)) {
                        Log.d(EventBus.TAG, "Skipping method (not public, static or abstract): " + clazz + "."
                                + methodName);
                    }
                }
            }
            clazz = clazz.getSuperclass();
        }
        if (subscriberMethods.isEmpty()) {
            throw new EventBusException("Subscriber " + subscriberClass + " has no public methods called "
                    + ON_EVENT_METHOD_NAME);
        } else {
            synchronized (methodCache) {
                methodCache.put(key, subscriberMethods);
            }
            return subscriberMethods;
        }
    }

register的下句:

	for (SubscriberMethod subscriberMethod : subscriberMethods) {
            subscribe(subscriber, subscriberMethod, sticky, priority);
    }

迭代所有查询到的订阅方法，并调用subscribe方法：

	// Must be called in synchronized block
    private void subscribe(Object subscriber, SubscriberMethod subscriberMethod, boolean sticky, int priority) {
        Class<?> eventType = subscriberMethod.eventType;
        CopyOnWriteArrayList<Subscription> subscriptions = subscriptionsByEventType.get(eventType);
        Subscription newSubscription = new Subscription(subscriber, subscriberMethod, priority);
        if (subscriptions == null) {
            subscriptions = new CopyOnWriteArrayList<Subscription>();
            subscriptionsByEventType.put(eventType, subscriptions);
        } else {
            if (subscriptions.contains(newSubscription)) {
                throw new EventBusException("Subscriber " + subscriber.getClass() + " already registered to event "
                        + eventType);
            }
        }

        // Starting with EventBus 2.2 we enforced methods to be public (might change with annotations again)
        // subscriberMethod.method.setAccessible(true);

        int size = subscriptions.size();
        for (int i = 0; i <= size; i++) {
            if (i == size || newSubscription.priority > subscriptions.get(i).priority) {
                subscriptions.add(i, newSubscription);
                break;
            }
        }

        List<Class<?>> subscribedEvents = typesBySubscriber.get(subscriber);
        if (subscribedEvents == null) {
            subscribedEvents = new ArrayList<Class<?>>();
            typesBySubscriber.put(subscriber, subscribedEvents);
        }
        subscribedEvents.add(eventType);

        if (sticky) {
            Object stickyEvent;
            synchronized (stickyEvents) {
                stickyEvent = stickyEvents.get(eventType);
            }
            if (stickyEvent != null) {
                // If the subscriber is trying to abort the event, it will fail (event is not tracked in posting state)
                // --> Strange corner case, which we don't take care of here.
                postToSubscription(newSubscription, stickyEvent, Looper.getMainLooper() == Looper.myLooper());
            }
        }
    }



    


分析EventBus源码，重复register而没有报错。  


此次记录EventBus register后一定要及时unregister，否则就可能会出现重复执行onEvent，甚至应用崩溃。
