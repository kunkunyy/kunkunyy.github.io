---
title: SVG 学习笔记（二）viewbox、height、width
date: 2023-11-08 19:28:07
tags:
- SVG 学习
categories:
- [前端学习笔记]
---

```HTML
<svg viewbox="0 0 100 100" width="100" height="100">
  ...
</svg>
```

# 基础

* viewBox：视窗盒子，简单理解就是在整个`svg`里截取某一块可见的区域
  * `viewBox="x, y, width, height" x: 左上角横坐标，y: 左上角纵坐标，width: 截取的宽度，height: 截取的长度`
  * 通过传递以上参数就可以确定区域。
* width，height: svg 面板的宽和高

# 难点

> **如何正确设置三者的值？**

需要明确需求是什么？------获取到我们需要/绘制的`svg`

* 如果说当前绘制的矢量图已经将整个svg空间填满，此时我们需要去拿到整个形状，此时将我们的`viewBox`完全覆盖整个svg面板即可。
```html
<svg width="24" height="24" viewBox="0 0 24 24">
  ...
</svg>
```

> 怎样在保持svg不变的前提下（不对内部path等元素进行修改）对svg进行放大缩小？

* 针对这个问题我们首先需要明确一点，`svg`的绘制是基于内部元素而定的，直接将`svg`的 `width、height、viewBox`等设置具体的数值其内部元素大小并不会跟着发生变化。
* 简单讲就是我们原来在一张 `4*4` 的纸画了一幅画，现在我们需要在 `8*8` 的纸上在 `4*4` 的范围内重新画一次，这样就导致只有 1/4 的面积有内容，其余部分均为空白。
* 解决方案：**不设置`svg`的宽高/设置宽高为 100%，然后用一个元素包裹该svg，通过设置父元素的宽高来控制svg的大小**

```html
<!--此时svg会撑满整个父元素-->
<div class="svg-container">
  <svg viewBox="0 0 24 24">...</svg>
</div>
```
```css
.svg-container{
  height: 100px;
  width: 100px;
}
```
