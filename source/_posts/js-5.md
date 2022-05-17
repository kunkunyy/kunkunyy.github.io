---
title: JavaScript学习笔记（五）数据类型概述
date: 2021-07-21 17:05:03
tags:
- JS学习
categories:
- [前端学习笔记]
- [JS]
---

# 数据类型

* ECMAScript有6种简单数据类型：{% post_link js-5-1 Undefined、Null、Boolean %}、{% post_link js-5-2 Number %}、{% post_link js-5-3 String %}、{% post_link js-5-4 Symbol %}。其中Symbol时ES6新增的数据类型
* {% post_link js-5-5 Object %}为复杂数据类型

# typeof操作符

* typeof是一个**操作符**，不是函数
~~~js
let message = "Hello World!";
console.log(typeof message);//string
console.log(typeof(message));//string
~~~
* 调用typeof null 返回的是"object"
    * 特殊值 null 被认为是一个对空对象的引用

## 返回值

当我们在使用typeof操作符来检测变量时会得到下列字符串之一：

| 字符串 | 意义 |
| :----: | :----: |
| underfined | 未定义 |
| boolean | 布尔值 |
| string | 字符串 |
| number | 数值 |
| object | 对象或null |
| function | 函数 |
| symbol | 符号 |

