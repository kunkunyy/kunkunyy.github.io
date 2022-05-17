---
title: CSS学习笔记（十二）用户体验（一）
date: 2022-01-28 18:51:01
tags:
- CSS学习
categories:
- [前端学习笔记]
- [CSS]
---

# 二十九、选用合适的鼠标光标

* 鼠标指针的用途不仅在于显示鼠标在屏幕上的位置，还可以告诉用户当前可以执行什么动作。

## 1、提示禁用状态

* 提示某个控件因为某些原因而变得无法交互（即控件已被禁用），用这个光标就再合适不过。

```css
:disabled, [disabled], [aria-disabled="true"] {
    cursor: not-allowed;
}
```

{%asset_img pic1.png # tu1%}

## 2、隐藏鼠标光标

```css
video {
    cursor: url(transparent.gif);
    cursor: none;
}
```

# 三十、扩大可点击区域

* 针对按钮或者选择框扩大可点击区域，提高用户体验感。
* 使用内嵌投影+外边框实现。

```css
button {
    border: 10px solid transparent;
    box-shadow: 0 0 0 1px rgba(0,0,0,.3) inset;
    background-clip: padding-box;
}
```

* box-shadow 可以同时指定多层投影。
* 伪元素同样可以代表其宿主元素来响应鼠标交互。

```css
button {
    position: relative;
    /* [其余样式] */
}
button::before {
    content: '';
    position: absolute;
    top: -10px; right: -10px;
    bottom: -10px; left: -10px;
}
```

# 三十一、自定义复选框

* 新的伪类 :checked。
    * 这个伪类只在复选框被勾选时才会匹配，不论这个勾选状态是由用户交互触发，还是由脚本触发。
* 可以基于复选框的勾选状态借助组合选择符来给其他元素设置样式。
* 当 ```<label>``` 元素与复选框关联之后，也可以起到触发开关的作用。

```css
input[type="checkbox"] {
	position: absolute;
	clip: rect(0,0,0,0);
}

input[type="checkbox"] + label::before {
	content: '\a0';
	display: inline-block;
	vertical-align: .2em;
	width: .8em;
	height: .8em;
	margin-right: .2em;
	border-radius: .2em;
	background: silver;
	text-indent: .15em;
	line-height: .65;
}

input[type="checkbox"]:checked + label::before {
	content: '\2713';
	background: yellowgreen;
}

input[type="checkbox"]:focus + label::before {
	box-shadow: 0 0 .1em .1em #58a;
}

input[type="checkbox"]:disabled + label::before {
	background: gray;
	box-shadow: none;
	color: #555;
	cursor: not-allowed;
}
```

{%asset_img pic2.png # tu1%}

## 开关式按钮

* 利用“复选框 hack”的思路来模拟。
* 只需要把 label 设置为按钮的样式即可，并不需要用到伪元素。

```css
input[type="checkbox"] {
    position: absolute;
    clip: rect(0,0,0,0);
}
input[type="checkbox"] + label {
    display: inline-block;
    padding: .3em .5em;
    background: #ccc;
    background-image: linear-gradient(#ddd, #bbb);
    border: 1px solid rgba(0,0,0,.2);
    border-radius: .3em;
    box-shadow: 0 1px white inset;
    text-align: center;
    text-shadow: 0 1px 1px white;
}
input[type="checkbox"]:checked + label,
input[type="checkbox"]:active + label {
    box-shadow: .05em .1em .2em rgba(0,0,0,.6) inset;
    border-color: rgba(0,0,0,.3);
    background: #bbb;
}
```

# 三十二、通过阴影来弱化背景

* 通过一层半透明的遮罩层来把后面的一切整体调暗，以便凸显某个特定的 UI 元素，引导用户关注。
* 最常见的实现方法就是增加一个额外的 HTML 元素用于遮挡背景，这个方法稳定可靠，但需要增加一个额外的 HTML 元素，这意味着该效果无法由 CSS 单独实现。

## 伪元素方案

* 用伪元素来消除额外的 HTML 元素。

```css
body.dimmed::before {
    position: fixed;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    z-index: 1;
    background: rgba(0,0,0,.8);
}
```

* **存在的问题：**
    * 这个方法的可移植性还不够好；因为 ```<body>```元素上可能有其他需求已经占用了 ::before 伪元素；
    * 在使用这个效果时，我们往往还需要一点 JavaScript 来给 ```<body>``` 添加 dimmed 这个类.
    * 伪元素无法绑定独立的 JavaScript 事件处理函数。

## box-shadow方案

* 对于简单的应用场景和产品原型来说，box-shadow 的扩张参数可以把元素的投影向各个方向延伸放大。
* **存在的问题：**
    * 由于遮罩层的尺寸是与视口相关，而不是与页面相关的，当我们滚动页面时，遮罩层的边缘就露出来了，除非给它加上 position: fixed;这个样式，或者页面并没有长到需要滚动的程度。
    * 当使用一个独立的元素（或伪元素）来实现遮罩层时，这个遮罩层不仅可以从视觉上把用户的注意力引导到关键元素上，还可以防止用户的鼠标与页面的其他部分发生交互，因为遮罩层会捕获所有指针事件。

```css
body {
    box-shadow: 0 0 0 50vmax rgba(0,0,0,.8);
}
```

## backdrop方案

* 如果你想引导用户关注的元素就是一个模态的 ```<dialog>``` 元素，那么根据浏览器的默认样式，它会自带一个遮罩层。
* 借助 ::backdrop 伪元素，这个原生的遮罩层也是可以设置样式的。

```css
dialog::backdrop {
    background: rgba(0, 0, 0, .8);
}
```

# 三十三、通过模糊来弱化背景

* 用一个额外的 HTML 元素来实现这个效果：
    * 需要把页面上除了关键元素之外的一切都包裹起来，这样就可以只对这个容器元素进行模糊处理了。
    * ```<main>``` 元素在这里是极为合适的。
* 结构代码基本上如下所示：

```html
<main>Bacon Ipsum dolor sit amet...</main>
<dialog>
    O HAI, I'm a dialog. Click on me to dismiss.
</dialog>
<!-- 其他对话框都写在这里 -->
```

* 每当弹出一个对话框，都需要给 ```<main>``` 元素增加一个类，以便对它应用模糊滤镜。

```css
main.de-emphasized {
    filter: blur(5px);
}
```
* 使用 brightness() 和 / 或 contrast() 滤镜：

```css
main.de-emphasized {
    filter: blur(3px) contrast(.8) brightness(.8);
}
```