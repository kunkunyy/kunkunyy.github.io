---
title: JavaScript学习笔记（三）JS语法基础
date: 2021-07-21 09:25:52
tags:
- JS学习
categories:
- [前端学习笔记]
- [JS]
---

# 标识符

## 定义

&emsp;&emsp;变量、函数、属性或函数参数的名称。

## 规则

### 命名规则

* 第一个字符必须是一个**字母、下划线或美元符号**
* 剩下字符可以是字母、下划线、美元符号或数字

### 其它

* 使用**小驼峰命名方式**（首字母小写，后面每个单词首字母大写）
* **关键字、保留字、true、false和null不能作为标识符**

# 严格模式

在脚本开头加上：```"use strict";```
```js
function message(){
    "use strict";
    //函数体
}
```

# 关键字和保留字

* 关键字：
{% asset_img pic1.png # tu %}

* 保留字：
{% asset_img pic2.png # tu %}