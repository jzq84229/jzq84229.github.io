---
layout: post
title: Annotation注解介绍
category: 技术
tags: annotation
keywords:
description:
---

## 元注解
  - @Target
  - @Retention
  - @Documented
  - @Inherited

### @Target
 说明了Annotation修饰的对象范围：Annotation可被用于 packages、types（类、接口、枚举、Annotation类型）、类型成员（方法、构造方法、成员变量、枚举值）、方法参数和本地变量（如循环变量、catch参数）。在Annotation类型的声明中使用了target可更加明晰其修饰的目标。

|值|说明|
|:---|:---|
|ElementType.ANNOTATION_TYPE     |注释类型声明|
|ElementType.CONSTRUCTOR         |构造方法申明|
|ElementType.FIELD               |字段声明（包括枚举常量）|
|ElementType.LOCAL_VARIABLE      |局部变量声明|
|ElementType.METHOD              |方法声明|
|ElementType.PACKAGE             |包声明|
|ElementType.PARAMETER           |方法的参数声明|
|ElementType.TYPE                |类、接口（包括注解类型）或枚举声明|


### @Retention
用于表明注解保留时长，即作用范围。如果注解的定义没有设置Retation，那默认会将其设置为RetentionPolicy.CLASS。
|值 |说明 |
|:---|:---|
|RetentionPolicy.SOURCE|源文件中有效，仅在源文件中保留，编译器会忽略注解|
|RetentionPolicy.CLASS|class文件中有效，编译器会保留注解，但jvm会忽略注解|
|RetentionPolicy.RUNTIME|运行时有效，在运行时jvm会保留注释，因此可以通过反射获取到注解的值|

### @Documented
用于描述其它类型的annotation应该被作为被标注的程序成员的公共API，因此可以被例如javadoc此类的工具文档化。Documented是一个标记注解，没有成员。

### @Inherited
@Inherited 元注解是一个标记注解，@Inherited阐述了某个被标注的类型是被继承的。如果一个使用了@Inherited修饰的annotation类型被用于一个class，则这个annotation将被用于该class的子类。

## 自定义注解


参考资料
---
[https://docs.oracle.com/javase/tutorial/java/annotations/index.html](https://docs.oracle.com/javase/tutorial/java/annotations/index.html)  
[http://www.cnblogs.com/peida/archive/2013/04/24/3036689.html](http://www.cnblogs.com/peida/archive/2013/04/24/3036689.html)  
[http://www.cnblogs.com/mandroid/archive/2011/07/18/2109829.html](http://www.cnblogs.com/mandroid/archive/2011/07/18/2109829.html)
