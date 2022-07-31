---
title: TypeScript学习笔记（五）联合类型
date: 2022-07-30 23:31:41
tags:
- TypeScript学习
categories:
- [前端学习笔记]
- [TypeScript]
---

# 定义

* 联合类型（Union Types）表示取值可以为**多种类型**中的一种。

# 使用方法

* 联合类型使用 | 分隔每个类型

# 例子

```ts
let myFavoriteNumber: string | number;
myFavoriteNumber = 'seven';
myFavoriteNumber = 7;
```

# 访问联合类型的属性或方法

* 当 TypeScript 不确定一个联合类型的变量到底是哪个类型的时候，我们****只能访问此联合类型的所有类型里共有的属性或方法**；
* 联合类型的变量在被赋值的时候，会根据类型推论的规则推断出一个类型

```ts
/*
    length 不是 string 和 number 的共有属性，所以会报错
    toString 是 string 和 number 的共有属性
*/

function getString(something: string | number): string {
    return something.toString();
}

function getLength(something: string | number): number {
    return something.length;
}
// index.ts(2,22): error TS2339: Property 'length' does not exist on type 'string | number'.
//   Property 'length' does not exist on type 'number'.

//----------------------------------------------------------------------

/*
    第二行的 myFavoriteNumber 被推断成了 string，访问它的 length 属性不会报错。
    第四行的 myFavoriteNumber 被推断成了 number，访问它的 length 属性时就报错了。
*/

let myFavoriteNumber: string | number;
myFavoriteNumber = 'seven';
console.log(myFavoriteNumber.length); // 5
myFavoriteNumber = 7;
console.log(myFavoriteNumber.length); // 编译时报错

// index.ts(5,30): error TS2339: Property 'length' does not exist on type 'number'.
```

