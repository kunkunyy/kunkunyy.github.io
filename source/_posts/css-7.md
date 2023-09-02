---
title: CSS学习笔记（七）形状（三）
date: 2022-01-10 17:28:57
tags: 
- CSS学习
categories:
- [前端学习笔记]
- [CSS]
---

# 十二、切角效果

* 使用渐变来达到目的。
* 一个角使用一层渐变，两个角使用两层渐变，以此类推。

```css
div{
    background: #58a;
    background:
        linear-gradient(-45deg, transparent 15px, #58a 0);
}
```

{%asset_img pic1.png # tu1%}

* 左下角和右下角的切角效果。

```css
div{
    background: #58a;
    background:
        linear-gradient(-45deg, transparent 15px, #58a 0) right,
        linear-gradient(45deg, transparent 15px, #655 0) left;
    background-size: 50% 100%;
    background-repeat: no-repeat;
}
```

{%asset_img pic2.png # tu1%}

* 四个角都切。

```css
div{
    background: #58a;
    background:
        linear-gradient(135deg, transparent 15px, #58a 0) top left,
        linear-gradient(-135deg, transparent 15px, #58a 0) top right,
        linear-gradient(-45deg, transparent 15px, #58a 0) bottom right,
        linear-gradient(45deg, transparent 15px, #58a 0) bottom left;
    background-size: 50% 50%;
    background-repeat: no-repeat;
}
```

{%asset_img pic3.png # tu1%}

* SCSS如下，可以直接调用，并传入2~5个参数。

```scss
@mixin beveled-corners($bg,$tl:0, $tr:$tl, $br:$tl, $bl:$tr) {
    background: $bg;
    background:
        linear-gradient(135deg, transparent $tl, $bg 0)
        top left,
        linear-gradient(225deg, transparent $tr, $bg 0)
        top right,
        linear-gradient(-45deg, transparent $br, $bg 0)
        bottom right,
        linear-gradient(45deg, transparent $bl, $bg 0)
        bottom left;
    background-size: 50% 50%;
    background-repeat: no-repeat;
}
```

## 弧形切角

* 使用径向渐变来替代上述线性渐变。

```css
div{
    background: #58a;
    background:
        radial-gradient(circle at top left,transparent 15px, #58a 0) top left,
        radial-gradient(circle at top right,transparent 15px, #58a 0) top right,
        radial-gradient(circle at bottom right,transparent 15px, #58a 0) bottom right,
        radial-gradient(circle at bottom left,transparent 15px, #58a 0) bottom left;
    background-size: 50% 50%;
    background-repeat: no-repeat;
}
```

{%asset_img pic4.png # tu1%}

## 内联SVG与border-image方案

略

## 裁切路径方案

```css
div{
    background: #58a;
    clip-path: polygon(
        20px 0, calc(100% - 20px) 0, 100% 20px,
        100% calc(100% - 20px), calc(100% - 20px) 100%,20px 100%, 0 calc(100% - 20px), 0 20px
    );
}
```

## 补充方案

* corner-shape属性。
* 为容器的四个角指定 15px 的斜面切角。

```css
div{
    border-radius: 15px;
    corner-shape: bevel;
}
```
