---
title: CSS学习笔记（三）背景与边框（三）
date: 2022-01-04 23:24:19
tags:
- CSS学习
categories:
- [前端学习笔记]
- [CSS]
---

# 六、复杂的背景图案

* **水平渐变图案跟垂直渐变图案配合实现简单的蓝图网格图案。**

## 0、知识点

* linear-gradient(direction, color-stop1 position, color-stop2  position, ...)：
    * 用于创建一个表示两种或多种颜色线性渐变的图片。
    * direction：用角度值指定渐变的方向（或角度）。
    * color-stop1, color-stop2,...：用于指定渐变的起止颜色。
* radial-gradient
* repeating-linear-gradient
* repeating-radial-gradient

## 1、网格

* **把多个渐变图案组合起来，将其透过彼此的透明区域显现。**

```css
div{
    background: white;
    background-image: linear-gradient(90deg,
                    rgba(200,0,0,.5) 50%, transparent 0),
                linear-gradient(
                    rgba(200,0,0,.5) 50%, transparent 0);
    background-size: 30px 30px;
}
```

{%asset_img pic1.png # tu1%}

* **网格中每个格子的大小可以调整，而网格线条的粗细同时保持固定。**

```css
div{
    background: #58a;
    background-image:
        linear-gradient(white 1px, transparent 0),
        linear-gradient(90deg, white 1px, transparent 0);
    background-size: 30px 30px;
}
```

{%asset_img pic2.png # tu1%}

* **把两幅不同线宽、不同颜色的网格图案叠加起来，得到一个更加逼真的蓝图网格。**

```css
div{
    background: #58a;
    background-image:
        linear-gradient(white 2px, transparent 0),
        linear-gradient(90deg, white 2px, transparent 0),
        linear-gradient(hsla(0,0%,100%,.3) 1px,transparent 0),
        linear-gradient(90deg, hsla(0,0%,100%,.3) 1px,transparent 0);
    background-size: 75px 75px, 75px 75px,
    15px 15px, 15px 15px;
}
```

{%asset_img pic3.png # tu1%}

## 2、波点

* **径向渐变创建最简单的圆点的阵列。**

```css
div{
    background: #655;
    background-image: radial-gradient(tan 30%, transparent 0);
    background-size: 30px 30px;
}
```

{%asset_img pic4.png # tu1%}

* **通过两层圆点阵列图案的错位排列可以实现真正的波点图案，第二个背景图片的偏移量正好等于贴片宽高的一半。**

```css
div{
    background: #655;
    background-image: 
        radial-gradient(tan 30%, transparent 0),
        radial-gradient(tan 30%, transparent 0);
    background-size: 30px 30px;
    background-position: 0 0, 15px 15px;
}
```

{%asset_img pic5.png # tu1%}

## 3、棋盘

* 棋盘图案是可以通过平铺生成的，平铺成这个图案的典型贴片包含两种不同颜色的方块，且相互间隔。
* 实现技巧在于用两个直角三角形来拼合出一个方块。
* 创建直角三角形渐变。

```css
div{
    background: #eee;
    background-image:
        linear-gradient(45deg, transparent 75%, #bbb 0);
    background-size: 30px 30px;
}
```

{%asset_img pic5.png # tu1%}

```css
div{
    background: #eee;
    background-image:
        linear-gradient(45deg, #bbb 25%, transparent 0),
        linear-gradient(45deg, transparent 75%, #bbb 0);
    background-size: 30px 30px;
}
```

{%asset_img pic6.png # tu1%}

* 把第二层渐变在水平和垂直方向均移动贴片长度的一半，把它们拼合成一个完整的方块。

```css
div{
    background: #eee;
    background-image:
        linear-gradient(45deg, #bbb 25%, transparent 0),
        linear-gradient(45deg, transparent 75%, #bbb 0);
    background-position: 0 0, 15px 15px;
    background-size: 30px 30px;
}
```

{%asset_img pic7.png # tu1%}

* 要把现有的这一组渐变重复一份，创建出另一组正方形，并且偏移它们的定位值。

```css
div{
    background: #eee;
    background-image:
        linear-gradient(45deg, #bbb 25%, transparent 0),
        linear-gradient(45deg, transparent 75%, #bbb 0),
        linear-gradient(45deg, #bbb 25%, transparent 0),
        linear-gradient(45deg, transparent 75%, #bbb 0);
    background-position: 0 0, 15px 15px,
                        15px 15px, 30px 30px;
    background-size: 30px 30px;
}
```

{%asset_img pic8.png # tu1%}

* 以把深灰色改成半透明的黑色，这样我们只需要修改底色就可以改变整个棋盘的色调，不需要单独调整各层渐变的色标了。

```css
div{
    background: #eee;
    background-image:
    linear-gradient(45deg,
        rgba(0,0,0,.25) 25%, transparent 0,
        transparent 75%, rgba(0,0,0,.25) 0),
    linear-gradient(45deg,
        rgba(0,0,0,.25) 25%, transparent 0,
        transparent 75%, rgba(0,0,0,.25) 0);
    background-position: 0 0, 15px 15px;
    background-size: 30px 30px;
}
```

{%asset_img pic9.png # tu1%}