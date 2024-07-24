---
title: TypeScript学习笔记（二）原始数据类型
date: 2022-07-29 14:40:10
cover: /img/article/winter-house.webp
tags:
- TypeScript学习
categories:
- [前端学习笔记]
- [TypeScript]
---

JavaScript 的类型分为两种：原始数据类型（Primitive data types）和对象类型（Object types）。

原始数据类型包括：布尔值、数值、字符串、null、undefined 以及 ES6 中的新类型 Symbol 和 ES10 中的新类型 BigInt。

# 布尔值

* 定义：boolean
* 构造函数Boolean创建的对象不是布尔值。

```ts
let test1: boolean = false;

let test2: boolean = new Boolean(1);
//报错

let test3: boolean = Boolean(1);
```

# 数值

* 定义：number
* ES6中的二进制和八进制表示法会被编译为十进制数字。

```ts
let test1: number = 1;
```

# 字符串

* 定义：string

```ts
let test1: string = 'Hello World!';
```

# 空值

* 定义：void
* 用于表示没有任何返回值的函数。

```ts
function alert(): void{
    alert('Hello World!');
}
```

# Null和Undefined

* 定义：null和undefined
* undefined和null是所有类型的子类型

```ts
let test1: undefined = undefined;

let test2: null = null;
```