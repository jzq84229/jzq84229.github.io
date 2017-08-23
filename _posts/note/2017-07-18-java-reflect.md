---
layout: post
title: 学习笔记——java反射
category: 笔记
tags:
keywords:
description:
---

### 实例化Class类对象
```
/**
 * 实例化Class类对象
 */
public static void getClassObject() {
    Class<?> demo1 = null;
    Class<?> demo2 = null;
    Class<?> demo3 = null;
    try {
        // 方式1： 推荐使用的Class实例化
        demo1 = Class.forName("com.zhang.reflect.bean.Demo");
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    }

    // 方式2： 通过Object类中的方法实例化
    demo2 = new Demo().getClass();
    // 方式3：类.class实例化
    demo3 = Demo.class;

    System.out.println("类名称：" + demo1.getName());
    System.out.println("类名称：" + demo2.getName());
    System.out.println("类名称：" + demo3.getName());
}
```

### 通过无参构造函数实例化Class定义的类的对象
```
/**
 * 通过无参构造函数实例化Class定义的类的对象
 */
public static void getClassInstance() {
    Class<?> demo = null;
    try {
        demo = Class.forName("com.zhang.reflect.bean.Person");
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    }
    Person per = null;
    try {
        per = (Person) demo.newInstance();
    } catch (InstantiationException e) {
        e.printStackTrace();
    } catch (IllegalAccessException e) {
        e.printStackTrace();
    }
    per.setName("Rollen");
    per.setAge(20);
    System.out.println(per);
}
```

### 反射构造函数实例化对象
```
/**
 * 反射构造函数实例化对象
 */
public static void getClassConstructors() {
    Class<?> demo = null;
    try {
        demo = Class.forName("com.zhang.reflect.bean.Person");
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    }
    Person per1 = null;
    Person per2 = null;
    Person per3 = null;
    Person per4 = null;
    Constructor<?> cons[] = demo.getConstructors();
    try {
        per1 = (Person) cons[0].newInstance();
        per2 = (Person) cons[1].newInstance("Rollen");
        per3 = (Person) cons[2].newInstance(20);
        per4 = (Person) cons[3].newInstance("Rollen", 20);
    } catch (InstantiationException e) {
        e.printStackTrace();
    } catch (IllegalAccessException e) {
        e.printStackTrace();
    } catch (InvocationTargetException e) {
        e.printStackTrace();
    }
    System.out.println(per1);
    System.out.println(per2);
    System.out.println(per3);
    System.out.println(per4);
}
```

### 反射获取类的接口、父类和构造函数
```
/**
 * 反射获取类的接口、父类和构造函数
 */
public static void getClassInterfaces() {
    Class<?> demo = null;
    try {
        demo = Class.forName("com.zhang.reflect.bean.People");
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    }
    //Class实现的所有接口
    Class<?> intes[] = demo.getInterfaces();
    for (int i = 0; i < intes.length; i++) {
        System.out.println("实现的接口：" + intes[i].getName());
    }
    //获取父类
    Class<?> temp = demo.getSuperclass();
    System.out.println("继承的父类为：" + temp.getName());

    //获取构造函数
    Constructor<?> cons[] = demo.getConstructors();
    for (int i = 0; i < cons.length; i++) {
        System.out.println("构造方法：" + cons[i]);
    }
}
```

### 反射获取所有构造函数
```
/**
 * 反射获取所有构造函数
 */
public static void showClassConstructors() {
    Class<?> demo = null;
    try {
        demo = Class.forName("com.zhang.reflect.bean.People");
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    }
    Constructor<?> cons[] = demo.getConstructors();
    for (int i = 0; i < cons.length; i++) {
        Class<?> p[] = cons[i].getParameterTypes();
        System.out.println("构造方法：");
        int mo = cons[i].getModifiers();
        System.out.print(Modifier.toString(mo) + " ");
        System.out.print(cons[i].getName());
        System.out.print("(");
        for (int j = 0; j < p.length; j++) {
            System.out.print(p[j].getName() + " arg" + i);
            if (j < p.length - 1) {
                System.out.print(",");
            }
        }
        System.out.println("){}");
    }
}
```

### 反射获取类全部方法
```
/**
 * 反射获取类全部方法
 */
public static void showClassMethods() {
    Class<?> demo = null;
    try {
        demo = Class.forName("com.zhang.reflect.bean.People");
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    }
    Method method[] = demo.getMethods();
    for (int i = 0; i < method.length; i++) {
        Class<?> returnType = method[i].getReturnType();
        Class<?> para[] = method[i].getParameterTypes();
        int temp = method[i].getModifiers();
        System.out.printf(Modifier.toString(temp) + " ");
        System.out.printf(returnType.getName() + " ");
        System.out.printf(method[i].getName() + " ");
        System.out.printf("(");
        for (int j = 0; j < para.length; j++) {
            System.out.printf(para[j].getName() + " " + "arg" + j);
            if (j < para.length - 1) {
                System.out.printf(",");
            }
        }
        Class<?> exce[] = method[i].getExceptionTypes();
        if (exce.length > 0) {
            System.out.printf(") throws ");
            for (int k = 0; k < exce.length; k++) {
                System.out.printf(exce[k].getName() + " ");
                if (k < exce.length - 1) {
                    System.out.printf(",");
                }
            }
        } else {
            System.out.printf(")");
        }
        System.out.println();
    }
}
```

### 反射类全部属性
```
/**
 * 显示类全部属性
 */
public static void showClassFields() {
    Class<?> demo = null;
    try {
        demo = Class.forName("com.zhang.reflect.bean.People");
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    }
    System.out.println("===============本类属性========================");
    //取得本类的全部属性
    Field[] fields = demo.getDeclaredFields();
    for (int i = 0; i < fields.length; i++) {
        //权限修饰符
        int mo = fields[i].getModifiers();
        String priv = Modifier.toString(mo);
        //属性类型
        Class<?> type = fields[i].getType();
        System.out.println(priv + " " + type.getName() + " " + fields[i].getName() + ";");
    }
    System.out.println("===============实现的接口或者父类的属性========================");
    // 取得实现的接口或者父类的属性
    Field[] fields1 = demo.getFields();
    for (int j = 0; j < fields1.length; j++) {
        //权限修饰符
        int mo = fields1[j].getModifiers();
        String priv = Modifier.toString(mo);
        //属性类型
        Class<?> type = fields1[j].getType();
        System.out.println(priv + " " + type.getName() + " " + fields1[j].getName() + ";");
    }
}
```

### 反射调用类中的方法
```
/**
 * 反射调用类中的方法
 */
public static void invokeClassMethod() {
    Class<?> demo = null;
    try {
        demo = Class.forName("com.zhang.reflect.bean.People");
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    }
    try {
        //调用People类中的sayChina方法
        Method method = demo.getMethod("sayChina");
        method.invoke(demo.newInstance());
        //调用People的sayHello方法
        method = demo.getMethod("sayHello", String.class, int.class);
        method.invoke(demo.newInstance(), "Rollen", 20);
    } catch (NoSuchMethodException e) {
        e.printStackTrace();
    } catch (IllegalAccessException e) {
        e.printStackTrace();
    } catch (InstantiationException e) {
        e.printStackTrace();
    } catch (InvocationTargetException e) {
        e.printStackTrace();
    }
}
```

### 反射调用类的get和set方法
```
/**
 * 反射调用类的get和set方法
 */
public static void invokeGetMethod() {
    Class<?> demo = null;
    Object obj = null;
    try {
        demo = Class.forName("com.zhang.reflect.bean.People");
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    }
    try {
        obj = demo.newInstance();
    } catch (InstantiationException e) {
        e.printStackTrace();
    } catch (IllegalAccessException e) {
        e.printStackTrace();
    }
    setter(obj, "Sex", "男", String.class);
    getter(obj, "Sex");
}

/**
 * 反射调用get方法
 * @param obj   操作对象
 * @param att   操作属性
 */
private static void getter(Object obj, String att) {
    try {
        Method method = obj.getClass().getMethod("get" + att);
        System.out.println(method.invoke(obj));
    } catch (NoSuchMethodException e) {
        e.printStackTrace();
    } catch (InvocationTargetException e) {
        e.printStackTrace();
    } catch (IllegalAccessException e) {
        e.printStackTrace();
    }
}

/**
 * 反射调用set方法
 * @param obj       操作对象
 * @param att       操作属性
 * @param value     设置的值
 * @param type      参数属性
 */
private static void setter(Object obj, String att, Object value, Class<?> type) {
    try {
        Method method = obj.getClass().getMethod("set" + att, type);
        method.invoke(obj, value);
    } catch (NoSuchMethodException e) {
        e.printStackTrace();
    } catch (InvocationTargetException e) {
        e.printStackTrace();
    } catch (IllegalAccessException e) {
        e.printStackTrace();
    }
}
```

### 通过反射操作属性
```
/**
 * 通过反射操作属性
 */
public static void setClassField() {
    Class<?> demo = null;
    Object obj = null;

    try {
        demo = Class.forName("com.zhang.reflect.bean.People");
        obj = demo.newInstance();

        Field field = demo.getDeclaredField("sex");
        field.setAccessible(true);
        field.set(obj, "男");
        System.out.println("sex:" + field.get(obj));
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    } catch (InstantiationException e) {
        e.printStackTrace();
    } catch (IllegalAccessException e) {
        e.printStackTrace();
    } catch (NoSuchFieldException e) {
        e.printStackTrace();
    }
}
```

### 通过反射取得并修改数组
```
/**
 * 通过反射取得并修改数组
 */
public static void modifyClassArray() {
    int[] temp = {1, 2, 3, 4, 5};
    Class<?> demo = temp.getClass().getComponentType();
    System.out.println("数组类型：" + demo.getName());
    System.out.println("数组长度：" + Array.getLength(temp));
    System.out.println("数组的第一个元素：" + Array.get(temp, 0));
    Array.set(temp, 0, 100);
    System.out.println("修改之后数组第一个元素为：" + Array.get(temp, 0));
}
```

### 通过反射修改数组大小
```
/**
 * 通过反射修改数组大小
 */
public static void modifyClassArraySize() {
    int[] temp = {1, 2, 3, 4, 5, 6, 7, 8, 9};
    int[] newTemp = (int[]) arrayInc(temp, 15);
    print(newTemp);
    System.out.println();
    System.out.println("===================");
    String[] atr = {"a", "b", "c"};
    String[] str1 = (String[]) arrayInc(atr, 8);
    print(str1);
}

/**
  * 修改数组大小
  * @param obj
  * @param len
  * @return
  */
 private static Object arrayInc(Object obj, int len) {
     Class<?> arr = obj.getClass().getComponentType();
     Object newArr = Array.newInstance(arr, len);
     int co = Array.getLength(obj);
     System.arraycopy(obj, 0, newArr, 0, co);
     return newArr;
 }

 /**
  * 打印
  * @param obj
  */
 private static void print(Object obj) {
     Class<?> c = obj.getClass();
     if (!c.isArray()) {
         return;
     }
     System.out.println("数组长度为：" + Array.getLength(obj));
     for (int i = 0; i < Array.getLength(obj); i++) {
         System.out.print(Array.get(obj, i) + " ");
     }
 }
```



参考资料：
---
[http://www.cnblogs.com/rollenholt/archive/2011/09/02/2163758.html](http://www.cnblogs.com/rollenholt/archive/2011/09/02/2163758.html)
