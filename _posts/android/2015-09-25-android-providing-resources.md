---
layout: post
title: Android屏幕适配-资源文件夹命名及匹配规则
category: Android
tags: Android
keywords:
description:
---

几乎所有的应用都要提另外的特殊资源来适配特殊配置的设备。例如：你必须提供不同尺寸的图片资源以适配不同屏幕密度的设备；你需要提供多个string资源文件以支持国际化。Android会根据当前设备的配置来加载适当的资源。

## 一. Android资源文件夹命名规则
android资源文件夹默认的命名见[Providing Resources][1]的table1

创建指定的具体资源文件：

1. 在res/文件夹下创建一个新的目录：<resources_name\>-<config_qualifier\>
	- <resources_name\>是默认的默认的目录名称（table1中定义的）
	- <qualifier\>限制条件（table2中定义）  
特殊资源目录名称可以有多个限制条件，每个限制条件用(-)短横线隔开。

> 警告:特殊资源目录名的多个限制条件必须按table2的优先级排列。如果限定条件的排列顺序错误，则编译不通过。  

2. 特殊资源目录中资源文件名称必须与默认资源目录下的资源文件名称相同。

Table2按优先级列出了所有限定条件。如果你的资源目录用到了多个限定条件，你必须按下表的名称顺命名。  
Table2. 限定条件名称(详见[Providing Resources][1]的table2)

|Configuration|Quanlifier Values|Description|
|:---|:---|:---|
|MCC and MNC|Examples:<br>mcc310<br>mcc310-mnc004<br>mcc208-mnc00<br>etc.|sim卡运营商|
|Language and region|Examples:<br>en<br>fr<br>en-rUS<br>fr-rFR<br>fr-rCA<br>etc.|语言与地区|
|Layout Direction|ldrtl<br>ldltr|布局方向|
|smallestWidth| sw\<N\>dp<br>Examples:<br>sw320dp<br>sw600dp<br>sw720dp<br>etc.|屏幕最小宽度|
|Available width|w\<N\>dp<br>Examples:<br>w720dp<br>w1024dp<br>etc.|屏幕最佳宽度|
|Available height|h\<N\>dp<br>Examples:<br>h720dp<br>h1024dp<br>etc.|屏幕最佳高度|
|Screen size|small<br>normal<br>large<br>xlarge|屏幕尺寸。<br>实际物理尺寸。就是我们常说的3.5英寸屏幕，4.7英寸屏幕等等，这个长度说的是对角线的长度。<br>在android中屏幕物理尺寸划分为这么几类：small，normal，large，extra large。<br>xlarge screens are at least 960dp x 720dp<br>large screen are at least 640dp x 480dp<br>normal screens are at least 470dp x 320dp<br>small screens are at least 426dp x 320dp<br>下图是对尺寸以及密度的一个粗略分类。<br>![Alt text](/assets/images/screens-ranges.png)|
|Screen aspect|long<br>notlong|屏幕长短边模式。<br>长宽比是屏幕的物理宽度与物理高度的比例关系。<br>long: Long screens, such as WQVGA, WVGA, FWVGA <br> notlong: Not long screens, such as QVGA, HVGA, and VGA|
|Round screen|round<br>notround|是否圆形屏幕<br>round: 圆形屏幕，如一些圆形穿戴设备<br>notround: 矩形屏幕，如手机、平板等|
|Screen orientation|port<br>land|当前屏幕横屏竖屏显示模式<br>port: 设备在纵向(vertical)<br>land: 设备在横向(horizontal)|
|UI mode|car<br>desk<br>television<br>appliance<br>watch|dock模式|
|Night mode|night<br>notnight|night: 晚上<br>notnight: 白天|
|Screen pixel density(dpi)|ldpi<br>mdpi<br>hdpi<br>xhdpi<br>xxhdpi<br>xxxhdpi<br>nodpi<br>tvdpi|屏幕最佳dpi<br>ldpi: ~120dpi<br>mdpi: ~160dpi<br>hdpi: ~240dpi<br>xhdpi: ~320dpi<br>xxhdpi: ~480dpi<br>xxxhdpi: ~640dpi<br>nodpi: 可用于不想随屏幕密度缩放的bitmap资源<br>tvdpi: 屏幕密度介于mdpi与hdpi之间，大约为213dpi。|
|Touchscreen type|notouch<br>finger|触摸屏类型|
|Keyboard availability|keysexposed<br>keyshidden<br>keyssoft|键盘类型|
|Primary text input method|nokeys<br>qwerty<br>12key|硬按键类型|
|Navigation key availability|navexposed<br>navhidden|方向键是否可用|
|Primary nontouch navigation method|nonav<br>dpad<br>trackball<br>whell|方向键类型|
|Platform Version(API level)|Examples:<br>v3<br>v4<br>v7<br>etc.|android版本|

> 注意：有些限定条件是从android 1.0就开始支持的，所以不是所有的Android版本都支持所有的限定条件。一个新版的限定条件，默认包含了android版本的限定条件，旧版Android会会自动忽略这个限定条件的资源文件夹。例如：values-w600dp的资源文件夹默认自动包含了v13的限定条件，因为available-width这个限定条件是从API v13才添加的。

### 资源文件夹命名规则：

- 资源文件夹名称可以指定多个限定条件并用（-）短横线分开。如：drawable-en-rUS-land表示美式英语的横向设备。
- 限定条件必须按table2的优先级排序。如：
	+ 错误：drawable-hdpi-port
	+ 正确：drawable-port-hdpi
- 资源目录不允许嵌套。如：不能有 res/drawable/drawable-en这样的目录。
- 资源目录不区分大小写。
- 同一限定条件只能有一个值。如：drawable-rES-rFR是不被允许的，你必须把它分成两个单独的资源目录，drawable-rES和drawable-rFR。

### 命名方法与要求

- 命名不区分大小写
- 命名形式：资源名-属性1-属性2-属性3-属性4-属性5……  
资源名就是table1的资源类型名，包括：drawable, values, layout, anim, raw, menu, color, animator, xml;  
属性1-属性2-属性3-属性4-属性5……就是table2的属性，如：en-port-hdpi;  

> 注意：各属性的位置顺序必须最受优先级从高到低排列，否则编译不通过。

### 实例说明

- 把全部属性都用上的例子（个属性按优先级先后排列出来的）  
values-mcc310-en-sw320dp-w720dp-h720dp-large-long-port-car-night-ldpi-notouch-keysexposed-nokeys-navexposed-nonav-v7
- 上述例子属性的中文说明  
values-mcc310(sim卡运营商)-en(语言)-sw320dp(屏幕最小宽度)-w720dp(屏幕最佳宽度)-h720dp(屏幕最佳高度)-large(屏幕尺寸)-long(屏幕长短边模式)-port(当前屏幕横竖屏显示模式)-car(dock模式)-night(白天或夜晚)-ldpi(屏幕最佳dpi)-notouch(触摸屏模类型)-keysexposed(键盘类型)-nokey(硬按键类型)-navexposed(方向键是否可用)-nonav(方向键类型)-v7(android版本)

## 二. 系统如何确定最佳资源

- 获取手机当前的基本配置信息（语言，横竖屏，屏幕密度，屏幕尺寸等等）
- 根据这些配置信息，排除apk包中与这些配置信息相矛盾的资源目录；如：系统语言为cn，那么所有的其他语言的目录都会被排除掉，注意系统并不会根据一个dpi的冲突而排除掉含有其他dpi的目录，dpi这个限制条件非常特殊。
- 按照限制条件的优先级，依次拿出配置信息中优先级最高的限制条件，去资源目录中寻找包含这个限制条件的目录，如果有，排除掉其他目录，根据配置中下一个优先级的限制条件继续匹配，如果没有，接着去拿下一个优先级的限制条件，继续寻找和排除，直到找到最合适的目录。

算法如下图:

![Alt text](/assets/images/res-selection-flowchart.png) ![Alt text](/assets/images/8e7d5a20-9315-377c-a9d4-3553485a0b6b.jpg)

### 实例
如：在一下资源目录中含有同一个文件名的图片资源。

	drawable/
	drawable-en/
	drawable-fr-rCA/
	drawable-en-port/
	drawable-en-notouch-12key/
	drawable-port-ldpi/
	drawable-port-notouch-12key/

并假设当前系统配置为：

	Locale = en-GB
	Screen orientation = port
	Screen pixel density = hdpi
	Touchscreen type = notouch
	Primary text input method = 12key

系统匹配最佳资源的过程为：
首先限制条件的优先级为：en-GB, port, hdpi, nottouch, 12key

1. 根据系统配置信息，首先排除drawable-fr-rCA 这个目录，因为fr和en冲突。  

drawable/  
drawable-en/  
<del>drawable-fr-rCA/</del>  
drawable-en-port/  
drawable-en-notouch-12key/  
drawable-port-ldpi/  
drawable-port-notouch-12key/  

> 注意：因为dpi使用match选择，所以drawable-port-ldpi没有被清除

2. 根据限制条件优先级，选择最高优先级限制条件(MCC最高，然后依次向下选择)
3. 是否有资源目录包含这个限制条件？
	- 没有，返回步骤2，并查看下一个限制条件。（本例中，没有符合的限制条件直到语言和地域）。
	- 有，继续步骤4。
4. 清除不包含此限制条件的所有目录。如：清除所有不包含语言的的目录：  

<del>drawable/</del>  
drawable-en/  
drawable-en-port/  
drawable-en-notouch-12key/  
<del>drawable-port-ldpi/</del>  
<del>drawable-port-notouch-12key/</del>  

> 注意：如果限定条件是屏幕像素密度，系统选择设备最匹配的屏幕密度。

5. 重复步骤2和3，直到步骤4中只剩下一个文件夹则返回。本例中，屏幕方向是下一个最高优先级的属性，所以可以清除两个文件夹：

<del>drawable-en/</del>  
drawable-en-port/  
<del>drawable-en-notouch-12key/</del>

最终的文件夹就是drawable-en-port/



参考资料
---
[http://developer.android.com/guide/topics/resources/providing-resources.html][1]   
[http://developer.android.com/guide/practices/screens_support.html](http://developer.android.com/guide/practices/screens_support.html)    
[http://ivan-ru.iteye.com/blog/1711414](http://ivan-ru.iteye.com/blog/1711414)   
[http://www.cnblogs.com/idealgrass/p/4271272.html](http://www.cnblogs.com/idealgrass/p/4271272.html)  


[1]:http://developer.android.com/guide/topics/resources/providing-resources.html
