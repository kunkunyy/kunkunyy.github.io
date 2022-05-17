---
title: CSS学习笔记（四）背景与边框（四）
date: 2022-01-08 15:07:26
tags:
- CSS学习
categories:
- [前端学习笔记]
- [CSS]
---

# 七、伪随机背景

* **通过质数来增加随机真实性**。

## 1、问题提出

* **自然界中的事物都不是以无限平铺的方式存在的。即使重复，也往往伴随着多样性和随机性。**
* 当你注意到一个有辨识度的特征（比如，木纹上的节疤）在以固定的规律循环重复时，那它试图营造的自然随机性就会立刻崩塌。
* 重现大自然的随机性是一个挑战，因为 CSS 本身没有提供任何随机功能。

## 2、解决方案

* **把这组条纹从一个平面拆散为多个图层：一种颜色作为底色，另三种颜色作为条纹，然后再让条纹以不同的间隔进行重复平铺。**
* 操作步骤：
    * 创建指定宽度的条纹；
    * 使用background-size来控制条纹的间距。

```css
#pic{
    background: hsl(20, 40%, 90%);
    background-image:
        linear-gradient(90deg, #fb3 10px, transparent 0),
        linear-gradient(90deg, #ab4 20px, transparent 0),
        linear-gradient(90deg, #655 20px, transparent 0);
    background-size: 80px 100%, 60px 100%, 40px 100%;
}
```

{%asset_img pic1.png # tu1%}

* 因为最顶层贴片的重复规律最容易被察觉（它没有被任何东西遮挡），我们应该把平铺间距最大的贴片安排在最顶层。
* **这个组合图案中第一个贴片的终点，就是各层背景图像以不同间距重复数次后再次统一对齐的点。**
* 因此，这里贴片的尺寸实际上就是所有 background-size 的最小公倍数，而 40、60 和 80的最小公倍数正是 240。
* **为了让最小公倍数最大化，这些数字最好是“相对质数”。**
* 要达成相对质数，最简单的办法就是**尽量选择质数**，因为质数跟其他任意数字都是相对质数。

```css
#pic{
    background: hsl(20, 40%, 90%);
    background-image:
        linear-gradient(90deg, #fb3 11px, transparent 0),
        linear-gradient(90deg, #ab4 23px, transparent 0),
        linear-gradient(90deg, #655 41px, transparent 0);
    background-size: 41px 100%, 61px 100%, 83px 100%;
}
```

{%asset_img pic2.png # tu1%}

# 八、连续的图像边框

## 1、提出问题

* 把一幅图案或图片应用为边框，而不是背景。
* 我们期望望出现在拐角处的图片区域是随着元素宽高和边框厚度的变化而变化的，并不想让图片的某个特定部分固定在拐角处。

{%asset_img pic4.png # tu1%}

* border-image 工作原理：
    * 九宫格伸缩法：把图片切割成九块，然后把它们应用到元素边框相应的边和角。
{%asset_img pic3.png # tu1%}

* 最简单的办法是使用两个 HTML 元素：**一个元素用来把我们的石雕图片设为背景，另一个元素用来存放内容，并设置纯白背景，然后覆盖在前者之上**：

```html
<div class="something-meaningful">
    <div>
        I have a nice stone art border,
        don't I look pretty?
    </div>
</div>
```
```css
.something-meaningful {
    background: url(stone-art.jpg);
    background-size: cover;
    padding: 1em;
}
.something-meaningful > div {
    background: white;
    padding: 1em;
}
```

* 如果只用一个元素，我们能做到这个效果吗？

## 2、解决问题

* 主要思路：**在石雕背景图片之上，再叠加一层纯白的实色背景。**
* 为了让下层的图片背景透过边框区域显示出来，我们需要给两层背景指定不同的 background-clip 值。
* 最后一个要点在于，我们只能在多重背景的最底层设置背景色，因此需要用一道从白色过渡到白色的 CSS 渐变来模拟出纯白实色背景的效果。

```html
<div class="something-meaningful">
    I have a nice stone art border,
    don't I look pretty?
</div>
```
```css
.something-meaningful {
    padding: 1em;
    border: 1em solid transparent;
    background: linear-gradient(white, white),
                url(stone-art.jpg);
    background-size: cover;
    background-clip: padding-box, border-box;
    background-origin: border-box;
}
```
* 这些新属性也是可以整合到 background 这个简写属性中的，这样可以显著地减少代码量：

```css
.something-meaningful {
    padding: 1em;
    border: 1em solid transparent;
    background:
        linear-gradient(white, white) padding-box,
        url(stone-art.jpg) border-box 0 / cover;
}
```

### 实现一个老式信封：

{%asset_img pic5.png # tu1%}

```css
div{
    padding: 1em;
    border: 1em solid transparent;
    background: linear-gradient(white, white) padding-box,
                repeating-linear-gradient(-45deg,
                red 0, red 12.5%,
                transparent 0, transparent 25%,
                #58a 0, #58a 37.5%,
                transparent 0, transparent 50%)
                0 / 5em 5em;
}
```
* 代码讲解：
* 创建两层背景：
    * linear-gradient(white, white) padding-box：用于创建第一层背景纯白色覆盖在最上层；
    * repeating-linear-gradient：用于创建重复线性梯度渐变。效果如下：
{%asset_img pic6.png # tu1%}

* repeating-linear-gradient中参数解释：
    * -45deg：用于将整个条纹旋转-45度；
    * red 0,red 12%：从0~12.5%位置均为红色；
    * transparent 0, transparent 25%：从上次的最大位置至25%的位置均为透明；
    * #58a 0, #58a 37.5%：从上次的最大位置至37.5%的位置均为#58；
    * transparent 0, transparent 50%：从上次的最大位置至50%的位置均为透明。
* 0/ ：background-position的值为0 0。
* 5em 5em：background-size大小。

### 蚂蚁行军效果

```css
@keyframes ants { to { background-position: 100% } }
div{
    padding: 1em;
    border: 1px solid transparent;
    background:
        linear-gradient(white, white) padding-box,
        repeating-linear-gradient(-45deg,
            black 0, black 25%, white 0, white 50%
        ) 0 / .6em .6em;
    animation: ants 12s linear infinite;
}
```

{%asset_img pic7.png # tu1%}