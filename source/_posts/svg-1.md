---
title: SVG 学习笔记（一）基础
date: 2023-11-08 19:28:07
tags:
- SVG 学习
categories:
- [前端学习笔记]
---

# 一、基本构成

* `<svg>`标签 + `<g>` 组合 + 各式形状
* SVG 里的**属性值必须用引号引起来，数值也同样**。
* 渲染顺序：**后来居上，越后面的元素越可见**。

# 二、SVG的引入

## 常规HTML

* 如果 HTML 是 XHTML 并且声明类型为 application/xhtml+xml，可以直接把 SVG 嵌入到 XML 源码中。
* SVG 可以直接被嵌入到 HTML 中。
```HTML
<html>
  <body>
    <h1>Inline SVG 示例</h1>
    <svg width="200" height="200">
      <circle cx="100" cy="100" r="50" fill="red" />
    </svg>
  </body>
</html>
```
* 可以使用 img 元素。
```HTML
<img src='./logo.svg'/>
```
* 可以通过 object 元素引用 SVG 文件：
```HTML
<object data="image.svg" type="image/svg+xml"></object>
```
* 使用 iframe 元素引用 SVG 文件
* 使用 CSS 的background-image属性
```HTML
<html>
  <head>
    <style>
      .svg-container {
        width: 200px;
        height: 200px;
        background-image: url("path/to/your-svg-file.svg");
        background-size: cover;
      }
    </style>
  </head>
  <body>
    <h1>外部SVG文件引入示例</h1>
    <div class="svg-container"></div>
  </body>
</html>
```