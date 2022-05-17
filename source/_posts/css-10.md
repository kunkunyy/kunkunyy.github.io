---
title: CSS学习笔记（十）字体排印（一）
date: 2022-01-15 21:31:28
tags:
- CSS学习
categories:
- [前端学习笔记]
- [CSS]
---

# 二十、连字符断行

* 在对文本进行两端对齐处理时，需要调整单词的间距，此时会出现“单词孤岛”现象。这个结果不仅看起来很糟糕，而且损伤了可读性。

```css
a {
    text-align: justify;
}
```

{%asset_img pic1.png # tu1%}

* 解决方案：
    * hyphens属性：接受三个值none、manual、auto。
    * 默认值为manual。

```css
a {
    hyphens: auto;
}
```

{%asset_img pic2.png # tu1%}

# 二十一、插入换行

* 由于```<dt>```、```<dd>```都是块级元素，当我们需要指定其在一行时会造成困难，如下所示：
* 我们需要创建以下格式列表：
{%asset_img pic3.png # tu1%}
* 应用如下代码和样式，会得到这样的效果。
```html
<dl>
    <dt>Name:</dt>
    <dd>Lea Verou</dd>
    <dt>Email:</dt>
    <dd>lea@verou.me</dd>
    <dt>Location:</dt>
    <dd>Earth</dd>
</dl>
```
```css
dd {
    margin: 0;
    font-weight: bold;
}
```
{%asset_img pic4.png # tu1%}

* 外加display:inline则会出现以下效果：

{%asset_img pic5.png # tu1%}

## 解决方案

* 利用伪元素来实现。
* 有一个 Unicode 字符是专门代表换行符的：0x000A。在 CSS 中，这个字符可以写作 "\000A"，或简化为 "\A"。
* 我们可以用它来作为 ::after 伪元素的内容，并将其添加到每个 ```<dd>``` 元素的尾部。
* **由于换行符会与空白符合并导致整体视觉效果压缩，所以需要保留换行符。**（如果不保留，则结果与上图没有任何变化）
* 使用white-space: pre实现以上结果。

## 实现步骤

* 针对单对单采取以下步骤：
    1、 将块级元素转换为行级元素；
    2、 利用伪元素给每个```<dd>```添加换行；
    3、 使用white-space:pre属性来处理空白。

```css
dt, dd { display: inline; }
dd {
    margin: 0;
    font-weight: bold;
}
dd::after {
    content: "\A";
    white-space: pre;
}
```

{%asset_img pic6.png # tu1%}

* 如果有一对多情况则需要调整一下第二步：
    * 在每个前面有```<dd>```的```<dd>```头部插入逗号；
    * 在每个前面有```<dd>```的```<dt>```头部插入换行。

```css
dt, dd { display: inline; }
dd {
    margin: 0;
    font-weight: bold;
}
dd + dt::before {
    content: '\A';
    white-space: pre;
}
dd + dd::before {
    content: ', ';
    font-weight: normal;
}
```

{%asset_img pic6.png # tu1%}

# 二十二、文本行的斑马条纹

## 知识点

* CSS 渐变；
* background-size；
* “条纹背景”；
* “灵活的背景定位”。

## 存在的问题

* 无法在文本行中应用斑马纹效果；

## 解决方案

* 利用背景图像来做到模拟斑马纹的效果。
* 直接上代码：

```css
div{
    padding: .5em;
    line-height: 1.5;
    background: beige;
    background-size: auto 3em;
    background-origin: content-box;
    background-image: linear-gradient(rgba(0,0,0,.2) 50%,transparent 0);
}
```

1、 在利用background设置整体背景色后，利用background-image属性创建渐变来实现不同行不同颜色的效果。
2、 这里需要设置background-size为行高line-height的两倍，因为每个背景贴片需要覆盖两行代码（一行有实际颜色，一行为透明）。
3、 padding属性将整体的文本调整位置不至于太过于靠边影响视觉效果。
4、 由于文本调整了位置，这里我们需要将背景进行些调整：让它的默认外沿padding box调整为content box。

{%asset_img pic7.png # tu1%}