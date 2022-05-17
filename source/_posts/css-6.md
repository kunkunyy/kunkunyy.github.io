---
title: CSS学习笔记（六）形状（二）
date: 2022-01-10 16:21:32
- CSS学习
categories:
- [前端学习笔记]
- [CSS]
---

# 十、平行四边形

## 提出问题

* 平行四边形其实是矩形的超集：它的各条边是两两平行的，但各个角则不一定都是直角。
* 有没有办法只让容器的形状倾斜，而保持其内容不变呢？

## 解决方案

### 嵌套元素方案

* 以对内容再应用一次反向的 skew() 变形，从而抵消容器的变形效果。

```html
<a href="#yolo" class="button">
    <div>Click me</div>
</a>
```
```css
.button { transform: skewX(-45deg); }
.button > div { transform: skewX(45deg); }
```

### 伪元素方案

* 把所有样式（背景、边框等）应用到伪元素上，然后再对伪元素进行变形。
* 一个简单的办法是给宿主元素应用 position: relative 样式，并为伪元素设置 position: absolute，然后再把所有偏移量设置为零，以便让它在水平和垂直方向上都被拉伸至宿主元素的尺寸。
* 同时设置z-index：-1样式来调整堆叠层次。

```css
.button {
    position: relative;
    /* 其他的文字颜色、内边距等样式…… */
}
.button::before {
    content: ''; /* 用伪元素来生成一个矩形 */
    position: absolute;
    top: 0; right: 0; bottom: 0; left: 0;
    z-index: -1;
    background: #58a;
    transform: skew(45deg);
}
```

# 十一、菱形图片

## 基于变形的方案

* 需要把图片用一个 div 包裹起来，然后对其应用相反的 rotate()变形样式：

```html
<div class="picture">
    <img src="adam-catlace.jpg" alt="..." />
</div>
```
```css
.picture {
    width: 400px;
    transform: rotate(45deg);
    overflow: hidden;
}
.picture > img {
    max-width: 100%;
    transform: rotate(-45deg);
}
```

* 以上代码并未实现最终效果：主要问题在于 max-width: 100% 这条声明。100% 会被解析为容器（.picture）的边长。
* **我们想让图片的宽度与容器的对角线相等，而不是与边长相等。**

{%asset_img pic1.png # tu1%}

* 通过scale()来缩放图片。

```css
.picture {
    width: 400px;
    transform: rotate(45deg);
    overflow: hidden;
}
.picture > img {
    max-width: 100%;
    transform: rotate(-45deg) scale(1.42);
}
```

{%asset_img pic2.png # tu1%}

### 裁切路径方案

* 使用clip-path属性和polygon()函数来指定一个菱形。

```css
.picture {
    clip-path: polygon(50% 0, 100% 50%, 50% 100%, 0 50%);
}
```

