---
title: TypeScript学习笔记（三）任意值
date: 2022-07-30 22:08:28
cover: /img/article/winter-river.webp
tags:
- TypeScript学习
categories:
- [前端学习笔记]
- [TypeScript]
---

# 定义

* 任意值用来表示允许赋值为任意类型。

```ts
let test1: any = 'seven';

test1 = 7;
```

# 任意值的属性和方法

* 在任意值上访问任何属性都是允许的，同时也允许调用任何方法。
* 声明一个变量为任意值之后，对它的任何操作，返回的内容的类型都是任意值。

```ts
let anyThing: any = 'hello';
console.log(anyThing.myName);
console.log(anyThing.myName.firstName);

let anyThing: any = 'Tom';
anyThing.setName('Jerry');
anyThing.setName('Jerry').sayHello();
anyThing.myName.setFirstName('Cat');
```

# 未声明类型的变量

* 变量如果在声明的时候，未指定其类型，那么它会被识别为任意值类型：

```ts
let something;
something = 'seven';
something = 7;

something.setName('Tom');
```