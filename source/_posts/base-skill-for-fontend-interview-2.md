---
title: 数据类型（二）包装类型
date: 2023-10-19 17:10:44
tags:
- 前端面试基本功
categories:
- [前端面试]
- [前端面试题]
---

```ts
const a = 18;
console.log(a.toString())// "18"
//===等价于===（js引擎做的事）
const a = 18;
const tmpAObject = new Number(a);
console.log(tmpAObject.toString())// "18"
```

# 包装类型与引用类型的不同

* 包装类型只会在执行瞬间被创建，调用后被销毁。

```ts
const person = {
  name: "zhangsan",
  age: 18
}
console.log(person.age)// 18
//======

```