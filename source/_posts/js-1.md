---
title: JavaScript学习笔记（一）JS概述
date: 2021-07-20 10:56:26
tags:
- JS学习
categories:
- [前端学习笔记]
- [JS]
- [ECMAScript]
- [DOM]
- [BOM]
---

# JavaScript实现

完整的 JavaScript 实现包含以下几个部分：
* 核心（ECMAScript）
* 文档对象模型（DOM）
* 浏览器对象模型（BOM）

## ECMAScript

* Web 浏览器只是 ECMAScript 实现可能存在的一种宿主环境（host environment）。
* ECMAScript 只是对实现这个规范描述的所有方面的一门语言的称呼。
* **ECMAScript符合性**
    * 支持 ECMA-262 中描述的所有“类型、值、对象、属性、函数，以及程序语法与语义”；
    * 支持 Unicode 字符标准。
    * 增加 ECMA-262 中未提及的“额外的类型、值、对象、属性和函数”。ECMA-262 所说的这些额外内容主要指规范中未给出的新对象或对象的新属性。
    * 支持 ECMA-262 中没有定义的“程序和正则表达式语法”（意思是允许修改和扩展内置的正则表达式特性）。

## DOM

* 定义：文档对象模型（DOM，Document Object Model）是一个应用编程接口（API），用于在 HTML 中使用扩展的 XML。
* **DOM 通过创建表示文档的树，让开发者可以随心所欲地控制网页的内容和结构。**


### DOM级别

* Level1：目标是映射文档结构
* Level2：增加了对（DHTML 早就支持的）鼠标和用户界面事件、范围、遍历（迭代 DOM 节点的方法）的支持，而且通过对象接口支持了层叠样式表（CSS）。
* Level3：增加了以统一的方式加载和保存文档的方法（包含在一个叫 DOM Load and Save 的新模块中），还有验证文档的方法（DOM Validation）

## BOM

* 定义：浏览器对象模型（BOM）API，用于支持访问和操作浏览器的窗口。
* 是唯一一个没有相关标准的 JavaScript 实现。
* BOM 主要针对浏览器窗口和子窗口（frame）
