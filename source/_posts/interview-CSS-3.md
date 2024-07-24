---
title: 前端面试题-CSS篇（三）
date: 2022-03-14 15:08:15
cover: /img/article/winter-house.webp
tags:
- JS学习
categories:
- [前端面试题]
- [CSS]
---

# 一、左侧固定，右侧自适应

```html
<div class="contioner">
    <div class="left"></div>
    <div class="main"></div>
</div>
```

## flex布局

* flex-basis 用于设置子项的占用空间。如果设置了值，则子项占用的空间为设置的值；如果没设置或者为 auto，那子项的空间为width/height 的值。
* flex-grow 用来“瓜分”父项的“剩余空间”。未设置默认为0
* flex-shrink用来“吸收”超出的空间。未设置默认为1。

```css
.contioner{
    display: flex;
}
.left{
    width: 200px;
    flex-shrink: 0;
}
.main{
    flex-grow: 1;
}
```

## grid布局

```css
.contioner{
    display: grid;
    grid-template-columns: 200px 1fr;
}
```

# 二、css画一个三角形

```html
<div class="triangle"></div>
```
```css
.triangle{
    width: 0;
    height: 0;
    border: 100px solid transparent;
    /*可以选择三角形朝向*/
    border-left: 100px solid rgb(66, 100, 100);
}
```

# 三、左右固定中间自适应

```html
<div class="contioner">
    <div class="left"></div>
    <div class="main"></div>
    <div class="right"></div>
</div>
```
```css
.contioner{
    display: flex;
}
.left{
    width: 300px;
    flex-shrink: 0;
}
.right{
    width: 300px;
    flex-shrink: 0;
}
.main{
    flex-grow: 1;
}
```