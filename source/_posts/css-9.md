---
title: CSS学习笔记（九）视觉效果
date: 2022-01-11 20:16:16
tags:
- CSS学习
categories:
- [前端学习笔记]
- [CSS]
---

# 十五、单侧投影

* 使用 box-shadow 的方法是，指定三个长度值和一个颜色值：

```css
div{
    box-shadow: 2px 3px 4px rgba(0,0,0,.5);
}
```

* box-shadow绘制原理：
    * 以选定的元素a为准，在相同的位置处绘制尺寸相同的元素b（a，b颜色不同）；
    * 将元素b进行移动，移动大小由参数决定（第一二个参数）；
    * 使用高斯模糊算法进行模糊处理（第三个参数决定）；
    * 模糊后的元素b与元素a交集的部分会被切除掉。

* 如何只在元素的一侧设置投影。
    * box-shadow的第四个长度参数，这个参数会根据你指定的值去扩大或缩小投影的尺寸

```css
div{
    box-shadow: 0 5px 4px -4px black;
}
```

## 邻边投影

* 如何在元素的两条边上设置投影。
* **扩张半径应该设为模糊半径相反值的一半。**
* **两个偏移量它们的值需要大于或等于模糊半径的一半。**

```css
div{
    box-shadow: 3px 3px 6px -3px black;
}
```

## 双侧投影

* 将投影设置在元素的两条对边。
* 将单侧投影使用两次来达到效果

```css
div{
    box-shadow: 5px 0 5px -5px black,
                -5px 0 5px -5px black;
}
```

# 十六、不规则投影

* 使用 drop-shadow 滤镜实现投影，任何非透明的部分都会被一视同仁地打上投影，包括文本，并且无法通过text-shadow:none; 来取消掉文本上的投影。

# 十七、染色效果

## 基于滤镜的方案

* 叠加滤镜效果的方式实现染色效果。
* sepia()：它会给图片增加一种降饱和度的橙黄色染色效果，几乎所有像素的色相值会被收敛到 35~40。
* saturate()：他会给每个像素提升饱和度。
* hue-rotate()：把每个像素的色相以指定的度数进行偏移。

```css
img{
    filter: sepia(1) saturate(4) hue-rotate(295deg);
}
```

{%asset_img pic1.png # tu1%}
{%asset_img pic2.png # tu1%}

## 基于混合模式的方案

* “混合模式”控制了上层元素的颜色与下层颜色进行混合的方式。
* 用它来实现染色效果时，需要用到的混合模式
是 luminosity。
    * luminosity 混合模式会保留上层元素的 HSL 亮度信息，并从它的下层吸取色相和饱和度信息。
* 要对一个元素设置混合模式，有两个属性可以派上用场：
    * mix-blend-mode 可以为整个元素设置混合模式；mix-blend-mode 是把整个元素向下进行混合，而不管它的下层是什么。
    * background-blend-mode 可以为每层背景单独指定混合模式。

```html
<a href="#something">
    <img src="tiger.jpg" alt="Rawrrr!" />
</a>
```

```css
a {
    background: hsl(335, 100%, 50%);
}
img {
    mix-blend-mode: luminosity;
}
```

```html
<div class="tinted-image"
    style="background-image:url(tiger.jpg)">
</div>
```

```css
.tinted-image {
    width: 640px; height: 440px;
    background-size: cover;
    background-color: hsl(335, 100%, 50%);
    background-blend-mode: luminosity;
    transition: .5s background-color;
}
.tinted-image:hover {
    background-color: transparent;
}
```

# 十八、毛玻璃效果

* 半透明颜色背景：

```html
<main>
    <blockquote>
        "The only way to get rid of a temptation[...]"
        <footer>－
            <cite>
                Oscar Wilde,
                The Picture of Dorian Gray
            </cite>
        </footer>
    </blockquote>
</main>
```
```css
body {
    background: url("tiger.jpg") 0 / cover fixed;
}
main {
    background: hsla(0,0%,100%,.3);
}
```

{%asset_img pic3.png # tu1%}

## 解决方案

* 由于我们不能直接对元素本身进行模糊处理，就对一个伪元素进行处理，然后将其定位到元素的下层，它的背景将会无缝匹配 ```<body>``` 的背景。
* 首先，我们添加一个伪元素，将其绝对定位，并把所有偏移量置为 0，这样就可以将它完整地覆盖到 ```<main>``` 元素之上：

```css
main {
    position: relative;
    /* [其余样式] */
}
main::before {
    content: '';
    position: absolute;
    top: 0; right: 0; bottom: 0; left: 0;
    background: rgba(255,0,0,.5); /* 仅用于调试 */
}
```

{%asset_img pic4.png # tu1%}

* 使用 z-index: -1; 来把伪元素移动到宿主元素的后面。

{%asset_img pic5.png # tu1%}

* 现在该把半透明红色背景换掉了，换成跟背层完全匹配的背景。要实现这一点，我们要么把 ```<body>``` 的背景复制过来，要么把伪元素的背景声明合并过去。

```css
body, main::before {
    background: url("tiger.jpg") 0 / cover fixed;
}
main {
    position: relative;
    background: hsla(0,0%,100%,.3);
}
main::before {
    content: '';
    position: absolute;
    top: 0; right: 0; bottom: 0; left: 0;
    filter: blur(20px);
}
```

{%asset_img pic6.png # tu1%}

* 模糊效果在中心区域看起来非常完美，但在接近边缘处会逐渐消退。这是因为模糊效果会削减实色像素所能覆盖的范围，削减的幅度正是模糊半径的长度。

{%asset_img pic7.png # tu1%}

* 为了补偿这种情况，我们需要让伪元素相对其宿主元素的尺寸再向外扩大至少 20px（即它的模糊半径）。可以通过 -20px 的外边距来达到目的，由于不同浏览器的模糊算法可能存在差异，用一个更大的绝对值（比如 -30px）会更保险一些。
* 这个方法可以修复边缘模糊消退的问题，但现在的情况是有一圈模糊效果超出了容器，这让它看起来不像毛玻璃，而更像是玻璃脏了。

{%asset_img pic8.png # tu1%}

* 不过幸运的是，这个问题也很容易修复：只要对 main 元素应用overflow: hidden;，就可以把多余的模糊区域裁切掉了。

```css
body, main::before {
    background: url("tiger.jpg") 0 / cover fixed;
}
main {
    position: relative;
    background: hsla(0,0%,100%,.3);
    overflow: hidden;
}
main::before {
    content: '';
    position: absolute;
    top: 0; right: 0; bottom: 0; left: 0;
    filter: blur(20px);
    margin: -30px;
}
```

{%asset_img pic9.png # tu1%}