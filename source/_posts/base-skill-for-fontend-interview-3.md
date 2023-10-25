---
title: 数据类型（三）判断数组的方式
date: 2023-10-19 17:18:20
tags:
- 前端面试基本功
categories:
- [前端面试]
- [前端面试题]
---

# Object.prototype.toString.call()

* Object.prototype.toString.call()，返回值是一个`[object [[class]]]`
* 每一个对象都有一个内部属性`[[class]]`，用于表示其类型，不可直接访问
  * 数组：Array
  * 方法：Function
  * 数字：Number
  * 字符串：String
  * 兑现：Object

# 通过原型链判断

```js
console.log([].__proto__ === Array.prototype)
```

# Array.isArray()

# instanceof

```js
console.log([] instanceof Array)
```