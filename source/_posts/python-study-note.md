---
title: python学习笔记（一）
date: 2021-03-15 14:46:30
categories:
- python
tags:
- python
- study notes
comment: true
---

    本科学习python课程笔记，之前使用的有道云笔记记载，现在将它搬运到博客中。
    第一章是一些基本概念，所以从第二章开始记的。所以，一起加油咯！
# 第二章

主要是turtle库的使用，然后简单介绍了python中的字符串的使用，第三章会详细讲字符串。

## 字符串简单介绍

### 字符串的两种序号表达

* 假设字符串长度为L：
    - 正向递增序号以最左侧字符序号为0，向右依次递增，最右侧字符序号为L-1；
    - 反向递减序号以最右侧字符序号为-1，向左依次递减，最左侧字符序号为-L。
* {% asset_img p1.png first %}

### 字符串截取范围

* TempStr[0:-1]    从0到-1，但不包括-1
* **口诀：前取后不取**

## turtle库

### 介绍

&emsp; &emsp; 实际上我们使用turtle库来绘制图形就是：在操纵“小海龟”在我们所定义的画布（canvas）（或者叫做窗体也可以）上进行爬行，它行动留下的痕迹就是我们所要绘制图形的轮廓。
&emsp; &emsp; 在了解绘制原理后我们就对于这个必不可少的画布进行了解和学习。

### 画布

#### 坐标轴

&emsp; &emsp; 一般情况下，当你创建了一个画布它就会对应的生成坐标轴，一方面是为了方便我们绘制图形；另一方面也是为了避免造成歧义导致出错。
&emsp; &emsp; 下图是对应坐标系的规定：
{% asset_img p2.png second %}
{% asset_img p3.png third %}

#### 画布（窗体）的创建

* turtle.screensize(canvwidth=None,canvheight=None,bg=None)
    - 参数：画布宽、高、背景色
* turtle.setup(width, height, startx, starty)
    - 参数：窗体宽、高、窗体左上角顶点的横纵坐标
    - 输入宽和高为整数时,表示像素;为小数时,表示占据电脑屏幕的比例

### 画笔

&emsp; &emsp;默认情况下，我们的“画笔”是一个位于坐标原点面朝正向的小海龟，我们通过控制小海龟来完成图形的绘制。

#### 画笔的属性

* turtle.pensize/width(width)
    - 用于设置画笔宽度
* turtle.pencolor()
    - 用于设置画笔颜色
* turtle.speed(x)
    - 用于设置画笔速度，0≤x≤10

#### 画笔移动状态

* turtle.forward/backward(d)
    - 向前/向后移动距离为d
* turtle.right/left(r)
    - 向左/右转动度数
* turtle.down/up()
    - 画笔落下/抬起
* turtle.fillcolor()
    - 给绘制的图形填充颜色
* turtle.circle(r,angle)
    - 绘制圆，半径正负均可，angle为角度
    - {% asset_img p4.png forth %}
    - {% asset_img p5.png fifth %}
* turtle.done()
    - 结束程序