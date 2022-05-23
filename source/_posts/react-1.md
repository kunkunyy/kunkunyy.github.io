---
title: React学习笔记（一）前言
date: 2022-05-17 20:16:36
tags:
- React学习
categories:
- [前端学习笔记]
- [React]
---

* React是一个将数据渲染为HTML视图的开源JavaScript库。
    * 数据→视图

# React特点

* **采用组件化模式、声明式编码**，提高开发效率和组件的复用率。
* 在React Native中可以使用React语法进行移动端开发。
* 使用**虚拟dom** + **Diffing算法**，尽量减少与真实dom操作。

# 虚拟DOM和真实DOM

* React提供了一些API来创建一种 “特别” 的一般js对象:
    * ```const VDOM = React.createElement('xx',{id:'xx'},'xx')```
    * 上面创建的就是一个简单的虚拟DOM对象；
* 虚拟DOM对象最终都会被React转换为真实的DOM；
* 只需要操作react的虚拟DOM相关数据, react会转换为真实DOM变化而更新界。

# Hello React

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hello React</title>
</head>
<body>
    <div id="test"></div>
    <script type="text/javascript" src="../js/react.development.js"></script>
    <script type="text/javascript" src="../js/react-dom.development.js"></script>
    <script type="text/javascript" src="../js/babel.min.js"></script>
    
    <script type="text/babel">
        // 创建虚拟dom
        const VDOM = <h1>Hello World!</h1>;
        // 渲染虚拟dom
        ReactDOM.render(VDOM,document.getElementById('test'));
    </script>
</body>
</html>
```

# 虚拟DOM的创建方式

* JSX相当于原生JS的语法糖。

## 1、使用JSX创建（推荐使用）

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hello React</title>
</head>
<body>
    <div id="test"></div>
    <script type="text/javascript" src="../js/react.development.js"></script>
    <script type="text/javascript" src="../js/react-dom.development.js"></script>
    <script type="text/javascript" src="../js/babel.min.js"></script>
    
    <script type="text/babel">
        // 创建虚拟dom
        const VDOM =(
            <h1 id="title">
                <span>Hello World!</span>
            </h1>
        );
        // 渲染虚拟dom
        ReactDOM.render(VDOM,document.getElementById('test'));
    </script>
</body>
</html>
```

## 2、原生JS创建

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hello React</title>
</head>
<body>
    <div id="test"></div>
    <script type="text/javascript" src="../js/react.development.js"></script>
    <script type="text/javascript" src="../js/react-dom.development.js"></script>
    <!-- <script type="text/javascript" src="../js/babel.min.js"></script> -->
    
    <script type="text/javascript">
        // 创建虚拟dom
        const VDOM = React.createElement('h1',{id:'title'},React.createElement('span',{},'Hello React!'));
        // 渲染虚拟dom
        ReactDOM.render(VDOM,document.getElementById('test'));
    </script>
</body>
</html>
```

# 虚拟DOM与真实DOM

* 虚拟DOM是Object对象。
* 虚拟DOM相较于真实DOM上的方法较少。
* 虚拟DOM会被React转化为真实DOM渲染到页面上。

# JSX

* 全称:  JavaScript XML；
* react定义的一种类似于XML的JS扩展语法: JS + XML本质是：
    ```React.createElement(component, props, ...children)```方法语法糖。
* jsx语法规则
    * 定义虚拟DOM式时，不要写引号。
    * 标签中混入**JS表达式**时需要使用{}。
    * 样式的类名使用className。
    * 内联样式，要使用style={{key:value}}形式
    * 虚拟DOM必须只有一个根标签。
    * 标签必须闭合。
    * 关于标签首字母：
        * 小写字母开头，则将该标签转为html中同名元素，若无同名元素，则报错；
        * 大写字母开头，react就会渲染对应的组件，若没有则报错。

# 模块与组件、模块化与组件化的理解

## 模块

* 向外提供特定功能的js程序, 一般就是一个js文件。
* 为什么要拆成模块：随着业务逻辑增加，代码越来越多且复杂。
* 作用：复用js, 简化js的编写, 提高js运行效率。

## 组件

* 用来实现局部功能效果的代码和资源的集合。
* 为什么要用组件： 一个界面的功能更复杂。
* 作用：复用编码, 简化项目编码, 提高运行效率。

## 模块化

* 当应用的js都以模块来编写的, 这个应用就是一个模块化的应用。

## 组件化

* 当应用是以多组件的方式实现, 这个应用就是一个组件化的应用。