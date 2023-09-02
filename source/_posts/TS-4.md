---
title: TypeScript学习笔记（四）类型推论
date: 2022-07-30 22:36:56
tags:
- TypeScript学习
categories:
- [前端学习笔记]
- [TypeScript]
---

# 定义

* 如果没有明确的指定类型，那么 TypeScript 会依照类型推论（Type Inference）的规则推断出一个类型。
* 举例：

```ts
let myFavoriteNumber = 'seven';
//等价于
let myFavoriteNumber: string = 'seven';
```

* 如果定义的时候没有赋值，不管之后有没有赋值，都会被推断成 any 类型而完全不被类型检查。

```ts
let myFavoriteNumber;
myFavoriteNumber = 'seven';
myFavoriteNumber = 7;
```