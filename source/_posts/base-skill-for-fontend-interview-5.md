---
title: 数据类型（四）null、undefined、NaN
date: 2023-10-19 17:19:02
tags:
- 前端面试基本功
categories:
- [前端面试]
- [前端面试题]
---

# null 和 undefined 的区别

* null 表示“无”的对象，转数字为 0 
  * 作为函数参数，表示该函数的参数不是对象
  * 作为对象原型链的终点
* undefined 表示“无”的原始值，转数字为 NaN
  * 表示缺少值，应该有值但是没定义
  * 变量被声明但未赋值
  * 函数没有返回值
  * 对象没有复制的属性

# typeof null结果时Object

* null是一种基本类型，在JavaScript设计之初是在32为系统，判断数据类型的时候是通过机器码进行的，每一种类型都有对应的机器码。
* 对象机器码为000，null机器码全为0，typeof在比较类型的时候只会比较后三位，两者相同，所以js认为两者相同。

# 如何安全获取 undefined 值

* void 0。

# isNaN 和 Number.isNaN

* isNaN
  * 会尝试讲参数进行类型转换，如果不能被转化为Number类型则会返回true。非数字值传入也会返回true
* Number.isNaN
  * 不会进行数据类型转换