---
title: CSS学习笔记（十三）结构与布局
date: 2022-02-03 00:34:01
cover: /img/article/winter-field.webp
tags:
- CSS学习
categories:
- [前端学习笔记]
- [CSS]
---

# 自适应内部元素

* 关键字：min-content
    * 将解析为这个容器内部最大的不可断行元素的宽度
* 把 figure 设置为恰当的宽度，并让它水平居中。

```css
figure {
    width: min-content;
    margin: auto;
}
```

# 精确控制表格列宽

* 对于不固定的内容，表格布局很难预测。
* table-layout属性：
    * 默认值是 auto，其行为模式被称作自动表格布局算法。
    * 接受另外一个值fixed，这个值的行为要明显可控一些。它把更多的控制权交给了网页开发者。

```css
table {
    table-layout: fixed;
    width: 100%;
}
```

# 根据兄弟元素的数量来设置样式

* 在某些场景下，我们需要根据兄弟元素的总数来为它们设置样式。
* 对于只有一个列表项的特殊场景来说，解决方案显然就是 :only-child。
* :only-child 等效于 :first-child:last-child。
```css
li:only-child {
    /* 只有一个列表项时的样式 */
}
```
* :last-child 其实也是一个快捷写法，相当于 :nth-last-child(1)。
* 这个 1 其实是一个参数，我们可以根据需要来修改这个值。
* li:first-child:nth-last-child(4)
    * 一个正好有四个列表项的列表中的第一个列表项。
    * 可以用兄弟选择符（~）来命中它之后的所有兄弟元素：相当于在这个列表正好包含四个列表项时，命中它的每一项。
```css
li:first-child:nth-last-child(4),
li:first-child:nth-last-child(4) ~ li {
 /* 当列表正好包含四项时，命中所有列表项 */
}
```

## 根据兄弟元素的数量范围来匹配元素

* :nth-child()可以用它来命中一个范围
    * 参数：an+b，n 表示一个变量，理论上的范围是 0 到 + ∞；
    * 如果使用 n+b 这种形式的表达式（此时相当于 a 的取值为 1），那么不论 n 如何取值，这个表达式都无法产生一个小于 b 的值。
    * 因此，n+b 这种形式的表达式可以选中从第 b 个开始的所有子元素。

# 满幅的背景，定宽的内容

```css
footer {
    padding: 1em;
    padding: 1em calc(50% - 450px);
    background: #333;
}
```

# 垂直居中

## 基于绝对定位的解决方案

* 它要求元素具有固定的宽度和高度。

```css
main {
    position: absolute;
    top: 50%;
    left: 50%;
    margin-top: -3em; /* 6/2 = 3 */
    margin-left: -9em; /* 18/2 = 9 */
    width: 18em;
    height: 6em;
}
```
或
```css
main {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}
```

## 基于视口单位的解决方案

```css
main {
    width: 18em;
    padding: 1em 1.5em;
    margin: 50vh auto 0;
    transform: translateY(-50%);
}
```

## 基于 Flexbox 的解决方案

* 只需写两行声明即可：先给这个待居中元素的父元素设置 display: flex，再给这个元素自身设置我们再熟悉不过的 margin: auto。

```css
body {
    display: flex;
    min-height: 100vh;
    margin: 0;
}
main {
    margin: auto;
}
```