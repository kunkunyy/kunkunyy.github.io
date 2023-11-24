---
title: style-component 学习笔记（一）介绍
date: 2023-11-09 19:12:50
tags:
- style-component
- css-in-js
categories:
- [style-component]
---

# 安装

```
npm install styled-components

yarn add styled-components
```

# 优点

* 自动关键 CSS：styled-components 会跟踪页面上呈现的组件，并完全自动地注入它们的样式，而不会注入其他样式。
* 没有类名错误：styled-components 会为你的样式生成唯一的类名。
* 更容易删除 CSS：要知道代码库中的某个类名是否被使用可能很难。styled-components 可以让这一点一目了然，因为每一个样式都与特定的组件绑定。如果组件未被使用（工具可以检测到）并被删除，其所有样式也会随之删除。
* 简单的动态样式：根据组件的道具或全局主题调整组件样式简单直观，无需手动管理数十个类。
* 无忧维护：你无需在不同的文件中寻找影响组件的样式，因此无论你的代码库有多大，维护都是小菜一碟。
* 自动供应商前缀：根据当前标准编写 CSS，其余的交给样式化组件处理。