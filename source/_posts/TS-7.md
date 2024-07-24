---
title: TypeScript学习笔记（七）数组类型
date: 2022-07-31 17:08:55
cover: /img/article/winter-piece.webp
tags:
- TypeScript学习
categories:
- [前端学习笔记]
- [TypeScript]
---

在 TypeScript 中，数组类型有多种定义方式，比较灵活。

# 「类型 + 方括号」表示法

* 数组的一些方法的参数也会根据数组在定义时约定的类型进行限制。

```ts
let fibonacci: number[] = [1, 1, 2, 3, 5];

let fibonacci: number[] = [1, '1', 2, 3, 5];
// Type 'string' is not assignable to type 'number'.

let fibonacci: number[] = [1, 1, 2, 3, 5];
fibonacci.push('8');

// Argument of type '"8"' is not assignable to parameter of type 'number'.
```
# 数组泛型

*  ```Array<elemType> ```来表示数组

```ts
let fibonacci: Array<number> = [1, 1, 2, 3, 5];
```

# 用接口表示数组

```ts
interface NumberArray {
    [index: number]: number;
}
let fibonacci: NumberArray = [1, 1, 2, 3, 5];
```

# 类数组

```ts
// 这个例子中，我们除了约束当索引的类型是数字时，值的类型必须是数字之外，也约束了它还有 length 和 callee 两个属性
function sum() {
    let args: {
        [index: number]: number;
        length: number;
        callee: Function;
    } = arguments;
}
```

# any 在数组中的应用

```ts
let list: any[] = ['xcatliu', 25, { website: 'http://xcatliu.com' }];
```