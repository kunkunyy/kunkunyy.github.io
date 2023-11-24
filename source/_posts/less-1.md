---
title: less 学习笔记
date: 2023-11-24 15:34:03
tags:
- less 学习笔记
categories:
- [前端]
- [CSS]
- [LESS]
---

# 简介

* `LESS` 是一个`CSS`预处理器，可为网站提供可定制、可管理和可重用的样式表。

# 变量

* 使用`@`定义一个变量，变量可以被复用。

```less
// less
@height: 100px;
@width: 200px;

.container {
  height: @height;
  width: @width;
}
//css
.container {
  height: 100px;
  width: 200px;
}
```

# 混入

* 混入是一种将一组属性从一个规则集中包含（"混合进入"）到另一个规则集中的方法。
* 例如现在有这样一个场景：现在有两个容器，除了背景色以外其余属性均相同

```less
//less
.commonContainer{
  width: 100px;
  height: 50px;
  border: 1.5px solid black;
}

.leftContainer{
  background-color: antiquewhite;
  .commonContainer()
}

.rightContainer{
  background-color: aquamarine;
  .commonContainer()
}

// css
.commonContainer {
  width: 100px;
  height: 50px;
  border: 1.5px solid black;
}
.leftContainer {
  background-color: antiquewhite;
  width: 100px;
  height: 50px;
  border: 1.5px solid black;
}
.rightContainer {
  background-color: aquamarine;
  width: 100px;
  height: 50px;
  border: 1.5px solid black;
}
```

# 嵌套规则

* 可以直接使用选择器进行嵌套

```less
// less
.container{
  display: flex;
  flex-direction: column;

  .first-child{
    background-color: red;
  }
  .second-child{
    background-color: blue;
  }
}
// css
.container {
  display: flex;
  flex-direction: column
}

.container .first-child {
  background-color: red
}

.container .second-child {
  background-color: blue
}
```

# 嵌套的 @ 规则和冒泡

* 诸如 `@media` 或 `@supports` 之类的 @ 规则可以以与选择器相同的方式嵌套。
* @ 规则放在最前面，与同一规则集中其他元素的相对顺序保持不变。这称为冒泡。

```less
// less
.component {
  width: 300px;
  @media (min-width: 768px) {
    width: 600px;
    @media  (min-resolution: 192dpi) {
      background-image: url(/img/retina2x.png);
    }
  }
  @media (min-width: 1280px) {
    width: 800px;
  }
}
// css
.component {
  width: 300px;
}
@media (min-width: 768px) {
  .component {
    width: 600px;
  }
}
@media (min-width: 768px) and (min-resolution: 192dpi) {
  .component {
    background-image: url(/img/retina2x.png);
  }
}
@media (min-width: 1280px) {
  .component {
    width: 800px;
  }
}
```

# 操作

* 算术运算 `+`、`-`、`*`、`/` 可以对任何数字、颜色或变量进行运算。
* **在计算时如果有单位，会优先考虑单位然后将数值进行转换然后进行计算。**
* 除法需要在括号中使用。

```less
// numbers are converted into the same units
@conversion-1: 5cm + 10mm; // result is 6cm
@conversion-2: 2 - 3cm - 5mm; // result is -1.5cm

// conversion is impossible
@incompatible-units: 2 + 5px - 3cm; // result is 4px

// example with variables
@base: 5%;
@filler: @base * 2; // result is 10%
@other: @base + @filler; // result is 15%
```

## calc() 异常

* 为了 CSS 兼容性，`calc()`不计算数学表达式，但会计算嵌套函数中的变量和数学。

```less
@var: 50vh/2;
width: calc(50% + (@var - 20px)); // result is calc(50% + (25vh - 20px))
```

# 函数

* `less` 有许多内置函数，可以转换颜色、操作字符串和进行数学运算。
* [函数参考手册](https://lesscss.cn/functions/)
* 使用它们非常简单。以下示例使用百分比将 `0.5` 转换为 `50%`，将基色的饱和度增加 `5%`，然后将背景颜色设置为亮 `25%` 并旋转 `8` 度的颜色：

```less
@base: #f04615;
@width: 0.5;

.class {
  width: percentage(@width); // returns `50%`
  color: saturate(@base, 5%);
  background-color: spin(lighten(@base, 25%), 8);
}
```

# 命名空间和访问器

* 有时，出于组织目的或只是为了提供一些封装，你可能希望对混入进行分组。 你可以在 Less 中非常直观地做到这一点。
* 假设你想在 `#bundle` 下打包一些混入和变量，以供以后重用或分发：

```less
#bundle() {
  .button {
    display: block;
    border: 1px solid black;
    background-color: grey;
    &:hover {
      background-color: white;
    }
  }
  .tab { ... }
  .citation { ... }
}
```

* 现在如果我们想在我们的 `#header a` 中混合 `.button` 类，我们可以这样做：

```less
#header a {
  color: orange;
  #bundle.button();  // can also be written as #bundle > .button
}
```

* 注意： 如果你不希望它出现在你的 `CSS` 输出中（即 `#bundle .tab`），请将 `()` 附加到你的命名空间（例如 `#bundle()`）。

# 映射

* 从 `Less 3.5` 开始，还可以使用混入和规则集作为值映射。

```less
// less
#colors() {
  primary: blue;
  secondary: green;
}

.button {
  color: #colors[primary];
  border: 1px solid #colors[secondary];
}
// css
.button {
  color: blue;
  border: 1px solid green;
}
```

# 作用域

* `Less` 中的作用域与 `CSS` 中的作用域非常相似。 首先在本地查找变量和混入，如果找不到，则从 "父级" 作用域继承。

```less
@var: red;

#page {
  @var: white;
  #header {
    color: @var; // white
  }
}
```

* 与 `CSS` 自定义属性一样，混入和变量定义不必放在引用它们的行之前。 所以下面的 `Less` 代码与前面的例子是一样的：

```less
@var: red;

#page {
  #header {
    color: @var; // white
  }
  @var: white;
}
```

# 注释

* 块式注释和行内注释都可以使用：

```less
/* One heck of a block
 * style comment! */
@var: red;

// Get in line!
@var: white;
```

# 导入

* 导入工作与预期的差不多。你可以导入一个 `.less` 文件，其中的所有变量都将可用。可选地为 `.less` 文件指定扩展名。

```less
@import "library"; // library.less
@import "typo.css";
```