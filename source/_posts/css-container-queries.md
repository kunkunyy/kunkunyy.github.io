---
title: CSS Container Queries(容器查询)
date: 2023-12-01 15:33:17
tags:
- CSS 学习
categories:
- [CSS]
---

# 提出问题

我们看一个如下图的新闻提要布局示例。新闻提要是一个文章的集合，每篇文章都有图像、标题和文本摘要三部分。页面右侧还有一个侧边栏，列出了热门文章。如果此时需要将整个页面进行划分你会如何去划分呢？

{%asset_img page.jpg # page%}

* **我们可以将其分为两个大的网格：左侧为 4 列网格，右侧为单列（侧边栏）。**
  * 左边：一篇横跨四栏的大型专题文章。
  * 下面：两篇文章，各占两栏。它们具有水平布局，图像位于左侧，文本位于右侧。
  * 下面：四篇较小的文章，每篇文章跨越一栏。右侧也会出现一栏相同风格的文章作为侧边栏。

{%asset_img grid.jpg # grid%}

我们可能会在这个布局中看到三种不同风格的文章，它们都将作为独立的组件进行开发。现在让我们看看较小视窗上（手机端）的设计会发生什么。

{%asset_img mobile.jpg # mobile%}

在手机尺寸下，**所有文章（包括特写文章）的图片都堆叠在文字上方。它们的布局没有区别**。尺寸稍大时，文章采用水平布局。在更大的视窗（可以认为是近似平板电脑大小的视窗）上，特写文章下有两篇横向文章，下面有四篇堆叠的文章。单篇文章总共有三种不同的布局。

如果使用媒体查询来查询视窗的大小，我们可能需要创建单独的组件来处理不同断点的不同文章布局的行为。我们的代码很容易变得有点笨重，而且难以维护。

## 将内容装进可用的空间中

* 想象一下，如果此时需求发生变化，需要一个单独的区域来放一些东西。我们的 5 列布局在可用空间中看起来不再美观，必须重新设计。在这种情况下，我们可以改变布局垂直堆叠一些文章，如下图。

{%asset_img page2.jpg # page2%}

* 如果我们使用媒体查询来检测视口宽度，我们的布局就无法响应可用空间的变化。此时就在想如果我们能查询父元素或容器的宽度，该多好啊。。。

# 什么是容器查询？

* 容器查询：**允许我们查询元素的大小（而不是视窗），并据此调整其后代的样式。**我们可以以类似于媒体查询的方式使用它们，但它们在布局方面为我们提供了更大的灵活性，并有可能大大简化我们的代码。
* 让我们以单个文章组件为例。我们将使用容器查询构建响应式文章组件布局。

{%asset_img article.jpg # article%}

## 1. 定义容器

{%asset_img code1.jpg # code1%}

* 废话不多说先来定义一个容器，如下代码：

```html
<ul class="grid">
  <li class="article-container">
    <article>...</article>
  </li>
  <li class="article-container">
    <article>...</article>
  </li>
  <li class="article-container">
    <article>...</article>
  </li>
  ...
</ul>

```

* 由于整体内容是一个文章列表，因此每篇文章的容器元素将是 `<li>`。为了避免混淆，我们将给它一个 `article-container` 类。

## 2. 定义容器的样式

* 有了容器后，就应该为其附加样式了，如下代码：

```css
.article-container {
  container-name: article;
  container-type: inline-size;
}
```

* 我们使用两个 CSS 属性来创建容器：容器名称（container-name）和容器类型（container-type）。
  * container-name：可选的，但如果页面上有多个容器，它就会非常有用，因为它可以让我们明确知道我们所指的是哪个容器。
  * container-type：对于基于大小的容器查询，容器类型是 `inline-size`。
    * `inline-size` 是一个逻辑值，因此它指的是使用水平书写模式（默认）时的宽度，或者垂直书写模式时的高度。
    * `size` 是另一个可选的值，但它指的是 CSS 包含模块。

> **简写：**`container: article / inline-size;`

## 3. 查询容器

* 一旦定义好了容器，编写容器查询就类似于编写媒体查询。在编写查询时，我们可以选择指定名称容器，也可以省略它，在这种情况下，查询将默认使用最接近的容器。
* 如果我们还没有定义容器，那么容器查询的行为将很像媒体查询——将查询视窗大小。

```css
/* 没有指定名称容器 */
@container (width > 700px) {
  /* 当容器宽度超过 700px 时，应用于任何容器内元素的样式  */
}

/* 具体到指定名称的容器 */
@container article (width > 700px) {
  /* 当容器宽度超过 700px 时，应用于 "文章 "容器内元素的样式  */
}
```