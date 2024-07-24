---
title: 前端面试题-CSS篇（一）
date: 2022-02-26 13:31:44
cover: /img/article/winter-pools.webp
tags:
- JS学习
categories:
- [前端面试题]
- [CSS]
---

# 一、盒子模型

* 标准盒子模型：
    * 标准盒子模型：宽度 = 内容的宽度(content) + border + padding + margin

![css-1-1](interview-CSS-1/css-1-1.jpg)

* IE盒子模型：
    * 宽度 = 内容宽度(content + border + padding) + margin

![css-1-2](interview-CSS-1/css-1-2.jpg)

* 盒模型：内容(content)、填充(padding)、边界(margin)、边框(border)。

## 1、页面渲染时,dom元素所采用的布局模型,可通过box-sizing进行设置。根据计算宽高的区域可分为:

* content-box (W3C标准盒模型) 
* border-box (IE盒模型) 
* padding-box (FireFox 曾经支持)
* margin-box (浏览器未实现)

```css
.div{
    box-sizing: content-box;
}
.div{
    box-sizing: border-box;
}
```

# 二、居中方法

## 1、水平居中

### text-align: center;

* 只针对行内元素。对于元素中的块级元素它是不起作用的。

```css
.text_div{
    text-align: center;
}
```

### margin: 0 auto;

* 针对块级元素的居中方案，必要前提：**需要知道父级元素的宽度**。

```css
.parent {
    width: 100%;
}
.children {
    margin: 0 auto;
}
```

### positon: absolute;

* 首先给父元素添加 position: relative 这样子元素就可以通过父元素来实现绝对定位。
* 然后让元素向右偏移 50%，需要注意，这个时候居中的是子元素的左边线。所以这时候就需要使用 translateX(-50%) 让元素再相对自己向右位移 50%，

```css
.parent {
    position: relative;
    width: 200px;
    height: 50px;
    border: 1px solid red;
}
.children {
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
    width: 50px;
    height: 50px;
    background: red;
}
```

### display: flex;

```css
.parent {
    display: flex;
    justify-content: center;
    width: 200px;
    height: 50px;
    border: 1px solid red;
}

.children {
    left: 50%;
    width: 50px;
    height: 50px;
    background: red;
}
```

## 垂直居中

### 单行内联元素垂直居中

* 通过设置内联元素的高度(height)和行高(line-height)相等，从而使元素垂直居中。

```css
.text_div {
    height: 50px;
    line-height: 50px;
}
```

### 利用表布局

```css
.parent {
    display: table;
}

.children {
    display: table-cell;
    vertical-align: middle;
    text-align: center;
}
```

### flex布局

```css
.center-flex{
    display: flex;
    flex-direction: column;
    justify-content: center;
}
```

### 绝对定位

* 已知元素高度：

```css
.parent {
    display: relative;
}

.children {
    display: absolute;
    top: 50%;
    height: 100px;
    margin-top: -50px;
}
```

* 未知元素高度：

```css
.parent {
    position: relative;
}

.children {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
}
```

## 垂直水平居中

### flex

```css
.parent {
    display: flex;
    justify-content: center;
    align-items: center
}
```

### grid

```css
.parent {
    display: grid;
}

.children {
    margin: auto;
}
```

### 绝对定位

```css
.parent{
    position: relative;
}
.child{
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%,-50%);
}
```

# 三、清除浮动

## 1、为什么需要清除浮动

* 不清楚浮动会发生高度塌陷：浮动元素父元素高度自适应。(父元素不写高度时，子元素写了浮动后，父元素会发生高度塌陷)

## 2、如何清除浮动

* 父级div定义height
* 最后一个浮动元素后加空div标签并添加样式clear:both。
* 包含浮动元素的父标签添加样式overflow为 hidden或auto。父级div定义zoo

# 四、CSS选择器

* **每个选择器都有权值，权值越大越优先**。
* 继承的样式优先级低于自身指定样式。
* ! important优先级最高js 也无法修改。
* 权值相同时，靠近元素的样式优先级高(顺序为内联样式表（标签内部>内部样式表（当前文件中)>外部样式表（外部文件中)

## 选择器类型

* 通配符选择器（*)
* 元素(类型)选择器
* 相邻选择器
* 属性选择器
* 伪类选择器
* 类选择器
* ID选择器
* 标签选择器

## 优先级算法如何计算？

* 元素选择符: 1 class 选择符:10；id选择符:100；元素标签:1000
1.!important声明的样式优先级最高，如果冲突再进行计算。
2.如果优先级相同，则选择最后出现的样式。
3.继承得到的样式的优先级最低。

## 选择器优先级

!important > 行内样式 > id > .class > tag > * > 继承 >默认选择器 从右往左解析

# 五、CSS3新增的伪类

* p:first-of-type 选择属于其**父元素的首个元素**的每个元素。
* p:last-of-type 选择属于其**父元素的最后元素**的每个元素。
* p:only-of-type 选择属于其**父元素唯一**的元素的每个元素。
* p:only-child选择属于其**父元素的唯一子元素**的每个元素。
* p:nth-child(2)选择属于其**父元素的第二个子元素**的每个元素。
* :enabled，:disabled 控制表单控件的禁用状态。
* :checked 单选框或复选框被选中。

# 六、边距重叠解决方案

* BFC(Block Formatting Context)块级格式化上下文，是用于布局块级盒子的一块渲染区域。
* 一个BFC的范围包含创建该上下文元素的所有子元素，但不包括创建了新BFC的子元素的内部元素。这从另一方角度说明，**一个元素不能同时存在于两个BFC中**。因为如果一个元素能够同时处于两个BFC中，那么就意味着这个元素能与两个BFC中的元素发生作用，就违反了BFC的隔离作用。

## 文档流

* 常规流：
    * 盒一个接着一个排列;
    * 在块级格式化上下文里面，它们竖着排列；
    * 在行内格式化上下文里面，它们横着排列;
    * 对于静态定位(static positioning)，position: static，盒的位置是常规流布局里的位置；
    * 对于相对定位(relative positioning)，position: relative，盒偏移位置由top、bottom、left、right属性定义。即使有偏移，仍然保留原有的位置，其它常规流不能占用这个位置。
* 浮动：
    * 左浮动元素尽量靠左、靠上，右浮动同理
    * **这导致常规流环绕在它的周边，除非设置 clear 属性**
    * 浮动元素不会影响块级元素的布局
    * 但浮动元素会影响行内元素的布局，让其围绕在自己周围，撑大父级元素，从而间接影响块级元素布局
    * 最高点不会超过当前行的最高点、它前面的浮动元素的最高点
    * 不超过它的包含块，除非元素本身已经比包含块更宽
    * 行内元素出现在左浮动元素的右边和右浮动元素的左边，左浮动元素的左边和右浮动元素的右边是不会摆放浮动元素的
* 绝对定位：
    * 绝对定位方案，盒从常规流中被移除，不影响常规流的布局；
    * 它的定位相对于它的包含块，相关CSS属性：top、bottom、left、right；
    * 如果元素的属性position为absolute或fixed，它是绝对定位元素；
    * 对于position: absolute，元素定位将相对于上级元素中最近的一个relative、fixed、absolute，如果没有则相对于body；

## BFC触发方式

* 根元素，即HTML标签
* 浮动元素：float值为left、right
* overflow值不为 visible，为 auto、scroll、hidden
* display值为 inline-block、table-cell、table-caption、table、inline-table、flex、inline-flex、grid、inline-grid
* 定位元素：position值为 absolute、fixed

## 约束规则

* 内部的Box会在垂直方向上一个接一个的放置
* 内部的Box垂直方向上的距离由margin决定。（完整的说法是：属于同一个BFC的两个相邻Box的margin会发生折叠，不同BFC不会发生折叠。）
* 每个元素的左外边距与包含块的左边界相接触（从左向右），即使浮动元素也是如此。（这说明BFC中子元素不会超出他的包含块，而position为absolute的元素可以超出他的包含块边界）
* BFC的区域不会与float的元素区域重叠
* 计算BFC的高度时，浮动子元素也参与计算
