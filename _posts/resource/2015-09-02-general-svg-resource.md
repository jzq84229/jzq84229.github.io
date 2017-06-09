---
layout: post
title: SVG 常用资源
category: 资源
tags: svg
keywords: svg
description:
---

[TOC]

**SVG意为可缩放矢量图形(Scalable Vector Graphics)。**

**SVG使用XML格式定义图形。**

**SVG 是使用 XML 来描述二维图形和绘图程序的语言。**

---

## SVG图形

SVG实例

	<?xml version="1.0" standalone="no"?>
	<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN"
	"http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">

	<svg width="100%" height="100%" version="1.1"
	xmlns="http://www.w3.org/2000/svg">

	<circle cx="100" cy="50" r="40" stroke="black"
	stroke-width="2" fill="red"/>

	</svg>


**代码解释：**

第一行包含了 XML 声明。请注意 standalone 属性！该属性规定此 SVG 文件是否是“独立的”，或含有对外部文件的引用。standalone="no" 意味着 SVG 文档会引用一个外部文件 - 在这里，是 [DTD][DTD] 文件。
第二和第三行引用了这个外部的 SVG DTD。该 DTD 位于 “http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd”。该 DTD 位于 W3C，含有所有允许的 SVG 元素。
SVG 代码以 <svg\> 元素开始，包括开启标签 <svg\> 和关闭标签 </svg\> 。这是根元素。width 和 height 属性可设置此 SVG 文档的宽度和高度。version 属性可定义所使用的 SVG 版本，xmlns 属性可定义 SVG [命名空间](http://www.w3school.com.cn/xml/xml_namespaces.asp "xml命名空间")。
SVG 的 <circle\> 用来创建一个圆。cx 和 cy 属性定义圆中心的 x 和 y 坐标。如果忽略这两个属性，那么圆点会被设置为 (0, 0)。r 属性定义圆的半径。  
stroke 和 stroke-width 属性控制如何显示形状的轮廓。我们把圆的轮廓设置为 2px 宽，黑边框。  
fill 属性设置形状内的颜色。我们把填充颜色设置为红色。  
关闭标签的作用是关闭 SVG 元素和文档本身。  

**SVG预定义形状**

* 矩形 <rect\>
* 圆形 <circle\>
* 椭圆 <ellipse\>
* 线 <line\>
* 折线 <polyline\>
* 多边形 <polygon\>
* 路径 <path\>

每种预定义形状都有自己的属性。

### 矩形rect

* x: 左上角x坐标
* y: 左上角y坐标
* rx: 圆角x坐标
* ry: 圆角y坐标
* width: 宽度
* height: 高度
* fill: 填充色
* stroke: 边框颜色
* stroke-width: 边框宽度
* fill-opacity: 填充透明度(0-1)
* stroke-opacity: 边框透明度(0-1)

所有属性可以通过style进行定义:

	<rect x="20" y="20" rx="20" ry="20" width="250"
	height="100" style="fill:red;stroke:black;
	stroke-width:5;opacity:0.5"/>

效果：

<svg width="100%" height="100%" version="1.1" xmlns="http://www.w3.org/2000/svg">
<rect x="5" y="5" width="250" height="100" style="fill:blue;stroke:pink;stroke-width:5;
fill-opacity:0.1;stroke-opacity:0.9" />
</svg>

### 圆形(circle)

* cx: 圆心x坐标
* cy: 圆形y坐标
* r: 圆的半径
* stroke
* stroke-width
* fill
* ...

实例：

	<circle cx="100" cy="50" r="40" stroke="black"
	stroke-width="2" fill="red"/>

效果：

<svg width="100%" height="100%" version="1.1" xmlns="http://www.w3.org/2000/svg">
<circle cx="100" cy="50" r="40" stroke="black"
stroke-width="2" fill="red" />
</svg>


### 椭圆(ellipse)

* cx: 圆点x坐标
* cy: 圆点y坐标
* rx: 水平半径
* ry：垂直半径
* ...

实例：

	<ellipse cx="240" cy="100" rx="220" ry="30"
	style="fill:purple"/>

	<ellipse cx="220" cy="70" rx="190" ry="20"
	style="fill:lime"/>

	<ellipse cx="210" cy="45" rx="170" ry="15"
	style="fill:yellow"/>

效果：

<svg width="100%" height="100%" version="1.1"
xmlns="http://www.w3.org/2000/svg">

<ellipse cx="240" cy="100" rx="220" ry="30"
style="fill:purple"/>

<ellipse cx="220" cy="70" rx="190" ry="20"
style="fill:lime"/>

<ellipse cx="210" cy="45" rx="170" ry="15"
style="fill:yellow"/>

</svg>

### 线条(line)

* x1:起始点x坐标
* y1:起始点y坐标
* x2:结束点x坐标
* y2:结束点y坐标
* ...

实例：

	<line x1="0" y1="0" x2="300" y2="300"
	style="stroke:rgb(99,99,99);stroke-width:2"/>

效果：

<svg width="100%" height="100%" version="1.1"
xmlns="http://www.w3.org/2000/svg">
<line x1="0" y1="0" x2="300" y2="300"
style="stroke:rgb(99,99,99);stroke-width:2"/>
</svg>

### 折线(polyline)

* points:点集
* ...

实例：

	<polyline points="0,0 0,20 20,20 20,40 40,40 40,60"
	style="fill:white;stroke:red;stroke-width:2"/>

效果：

<svg width="100%" height="100%" version="1.1"
xmlns="http://www.w3.org/2000/svg">
<polyline points="0,0 0,20 20,20 20,40 40,40 40,60" style="fill:white;stroke:red;stroke-width:2"/>
</svg>


### 多边形(polygon)

* points:多边形每个角的x和y坐标
* ...

实例：

	<polygon points="220,100 300,210 170,250 123,234"
	style="fill:#cccccc;
	stroke:#000000;stroke-width:1"/>

效果：

<svg width="100%" height="100%" version="1.1"
xmlns="http://www.w3.org/2000/svg">
<polygon points="220,100 300,210 170,250 123,234"
style="fill:#cccccc;
stroke:#000000;stroke-width:1"/>
</svg>


### 路径(path)

* d:内容数据
* ...

下面的命令可用于路径数据：

|命令|全称|含义|
|----|---|---|
|M | moveto| 移至|
|L | lineto| 直线至|
|H | horizontal lineto| 横向直线至|
|V | vertical lineto| 竖向直线至|
|C | curveto| 曲线至|
|S | smooth curveto| 平滑曲线至|
|Q | quadratic Belzier curve| 二次方贝塞尔曲线至|
|T | smooth quadratic Belzier curveto| 平滑二次方贝塞尔曲线至|
|A | elliptical Arc| 椭圆弧至|
|Z | closepath| 关闭路径|

> 注释：以上命令均允许小写字母。大写表示绝对定位， 小写表示相对定位。

实例：

    <path d="M250 150 L150 350 L350 350 Z" />

效果：

<svg width="100%" height="100%" version="1.1"
xmlns="http://www.w3.org/2000/svg">
<path d="M250 150 L150 350 L350 350 Z" />
</svg>

实例：

	<path d="M153 334
	C153 334 151 334 151 334
	C151 339 153 344 156 344
	C164 344 171 339 171 334
	C171 322 164 314 156 314
	C142 314 131 322 131 334
	C131 350 142 364 156 364
	C175 364 191 350 191 334
	C191 311 175 294 156 294
	C131 294 111 311 111 334
	C111 361 131 384 156 384
	C186 384 211 361 211 334
	C211 300 186 274 156 274"
	style="fill:white;stroke:red;stroke-width:2"/>

效果：

<svg width="100%" height="100%" version="1.1"
xmlns="http://www.w3.org/2000/svg">
<path d="M153 334
C153 334 151 334 151 334
C151 339 153 344 156 344
C164 344 171 339 171 334
C171 322 164 314 156 314
C142 314 131 322 131 334
C131 350 142 364 156 364
C175 364 191 350 191 334
C191 311 175 294 156 294
C131 294 111 311 111 334
C111 361 131 384 156 384
C186 384 211 361 211 334
C211 300 186 274 156 274"
style="fill:white;stroke:red;stroke-width:2"/>
</svg>


很复杂吧？是的！！！由于绘制路径的复杂性，因此强烈建议您使用 SVG 编辑器来创建复杂的图形。

## SVG滤镜

在 SVG 中，可用的滤镜有：

* feBlend
* feColorMatrix
* feComponentTransfer
* feComposite
* feConvolveMatrix
* feDiffuseLighting
* feDisplacementMap
* feFlood
* feGaussianBlur
* feImage
* feMerge
* feMorphology
* feOffset
* feSpecularLighting
* feTile
* feTurbulence
* feDistantLight
* fePointLight
* feSpotLight

> 注释：您可以在每个 SVG 元素上使用多个滤镜！

### 高斯模糊(Gaussian Blur)

<filter\>标签用来定义SVG滤镜。<filter\>标签使用必须的id属性来定义向图像应用哪个滤镜？  
<filter\>标签必须嵌套在<defs\>标签内。<defs\>标签是definitions的缩写，他允许对诸如滤镜等特殊元素进行定义。  

实例：

	<defs>
	<filter id="Gaussian_Blur">
	<feGaussianBlur in="SourceGraphic" stdDeviation="3" />
	</filter>
	</defs>

	<ellipse cx="200" cy="150" rx="70" ry="40"
	style="fill:#ff0000;stroke:#000000;
	stroke-width:2;filter:url(#Gaussian_Blur)"/>

**代码解释：**

* <filter\> 标签的 id 属性可为滤镜定义一个唯一的名称（同一滤镜可被文档中的多个元素使用）
* filter:url 属性用来把元素链接到滤镜。当链接滤镜 id 时，必须使用 # 字符
* 滤镜效果是通过 <feGaussianBlur\> 标签进行定义的。fe 后缀可用于所有的滤镜
* <feGaussianBlur\> 标签的 stdDeviation 属性可定义模糊的程度
* in="SourceGraphic" 这个部分定义了由整个图像创建效果

效果：

<svg width="100%" height="100%" version="1.1"
xmlns="http://www.w3.org/2000/svg">

<defs>
<filter id="Gaussian_Blur">
<feGaussianBlur in="SourceGraphic" stdDeviation="3" />
</filter>
</defs>

<ellipse cx="200" cy="150" rx="70" ry="40"
style="fill:#ff0000;stroke:#000000;
stroke-width:2;filter:url(#Gaussian_Blur)"/>

</svg>

## SVG 线形渐变

**SVG渐变必须在<defs>标签中进行定义。**

---

渐变是一种从一种颜色到另一种颜色的平滑过渡。另外，可以把多个颜色的过渡应用到同一个元素上。

在 SVG 中，有两种主要的渐变类型：

* 线性渐变
* 放射性渐变

### 线性渐变

<linearGradient\> 可用来定义 SVG 的线性渐变。  
<linearGradient\> 标签必须嵌套在 <defs> 的内部。<defs> 标签是 definitions 的缩写，它可对诸如渐变之类的特殊元素进行定义。  

线性渐变可被定义为水平、垂直或角形的渐变：

* 当 y1 和 y2 相等，而 x1 和 x2 不同时，可创建水平渐变
* 当 x1 和 x2 相等，而 y1 和 y2 不同时，可创建垂直渐变
* 当 x1 和 x2 不同，且 y1 和 y2 不同时，可创建角形渐变

实例：

	<defs>
	<linearGradient id="orange_red" x1="0%" y1="0%" x2="100%" y2="0%">
	<stop offset="0%" style="stop-color:rgb(255,255,0);
	stop-opacity:1"/>
	<stop offset="100%" style="stop-color:rgb(255,0,0);
	stop-opacity:1"/>
	</linearGradient>
	</defs>

	<ellipse cx="200" cy="190" rx="85" ry="55"
	style="fill:url(#orange_red)"/>

**代码解释：**

* <linearGradient\> 标签的 id 属性可为渐变定义一个唯一的名称
* fill:url(#orange_red) 属性把 ellipse 元素链接到此渐变
* <linearGradient\> 标签的 x1、y1、属性定义渐变开始位置，x2、y2 属性可定义渐变的结束位置
* 渐变的颜色范围可由两种或多种颜色组成。每种颜色通过一个 <stop> 标签来规定。offset 属性用来定义渐变的开始和结束位置。

效果：

<svg width="100%" height="100%" version="1.1"
xmlns="http://www.w3.org/2000/svg">

<defs>
<linearGradient id="orange_red" x1="0%" y1="0%" x2="100%" y2="0%">
<stop offset="0%" style="stop-color:rgb(255,255,0);
stop-opacity:1"/>
<stop offset="100%" style="stop-color:rgb(255,0,0);
stop-opacity:1"/>
</linearGradient>
</defs>

<ellipse cx="200" cy="190" rx="85" ry="55"
style="fill:url(#orange_red)"/>

</svg>

### 放射性渐变
<radialGradient\> 用来定义放射性渐变。  
<radialGradient\> 标签必须嵌套在 <defs> 中。<defs> 标签是 definitions 的缩写，它允许对诸如渐变等特殊元素进行定义。  

	<defs>
	<radialGradient id="grey_blue" cx="50%" cy="50%" r="50%"
	fx="50%" fy="50%">
	<stop offset="0%" style="stop-color:rgb(200,200,200);
	stop-opacity:0"/>
	<stop offset="100%" style="stop-color:rgb(0,0,255);
	stop-opacity:1"/>
	</radialGradient>
	</defs>

	<ellipse cx="230" cy="200" rx="110" ry="100"
	style="fill:url(#grey_blue)"/>

**属性：**

* <radialGradient\> 标签的 id 属性可为渐变定义一个唯一的名称
* fill:url(#grey_blue) 属性把 ellipse 元素链接到此渐变，
* cx、cy 和 r 属性定义外圈(渐变范围)
* 而 fx 和 fy 定义内圈(渐变结束点)
* 渐变的颜色范围可由两种或多种颜色组成。每种颜色通过一个 <stop> 标签来规定。offset 属性用来定义渐变的开始和结束位置。

效果：

<svg width="100%" height="100%" version="1.1"
xmlns="http://www.w3.org/2000/svg">

<defs>
<radialGradient id="grey_blue" cx="50%" cy="50%" r="50%"
fx="50%" fy="50%">
<stop offset="0%" style="stop-color:rgb(200,200,200);
stop-opacity:0"/>
<stop offset="100%" style="stop-color:rgb(0,0,255);
stop-opacity:1"/>
</radialGradient>
</defs>

<ellipse cx="230" cy="200" rx="110" ry="100"
style="fill:url(#grey_blue)"/>

</svg>


## SVG 实例
[SVG 实例](http://www.w3school.com.cn/svg/svg_examples.asp "SVG实例")

## SVG 元素
[svg 元素](http://www.w3school.com.cn/svg/svg_reference.asp "SVG元素")


参考资料：
---

[SVG教程 http://www.w3school.com.cn/svg/](http://www.w3school.com.cn/svg/)  
[DTD教程 http://www.w3school.com.cn/dtd/dtd_intro.asp](http://www.w3school.com.cn/dtd/dtd_intro.asp)  
[XML的基础和DOCTYPE字段的解析](http://lz12366.iteye.com/blog/725383)  

[DTD]: http://www.w3school.com.cn/dtd/dtd_intro.asp "DTD教程"
