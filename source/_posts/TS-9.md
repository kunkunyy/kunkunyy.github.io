---
title: TypeScript学习笔记（九）类型断言
date: 2022-08-01 10:33:12
tags:
- TypeScript学习
categories:
- [前端学习笔记]
- [TypeScript]
---

* 类型断言（Type Assertion）可以用来手动指定一个值的类型。

# 语法

```
值 as 类型

<类型>值
```

# 类型断言的限制

* 若 A 兼容 B，那么 A 能够被断言为 B，B 也能被断言为 A。

```ts
interface Animal {
    name: string;
}
interface Cat {
    name: string;
    run(): void;
}

let tom: Cat = {
    name: 'Tom',
    run: () => { console.log('run') }
};
let animal: Animal = tom;

// 等价于

interface Animal {
    name: string;
}
interface Cat extends Animal {
    run(): void;
}
```

# 双重断言

* ```as any as Foo```来将任何一个类型断言为任何另一个类型。
* 打破「要使得 A 能够被断言为 B，只需要 A 兼容 B 或 B 兼容 A 即可」的限制，将任何一个类型断言为任何另一个类型。

```ts
interface Cat {
    run(): void;
}
interface Fish {
    swim(): void;
}

function testCat(cat: Cat) {
    return (cat as any as Fish);
}
```

# 比较

* 类型断言 vs 类型转换
    * 类型断言只会影响 TypeScript 编译时的类型，类型断言语句在编译结果中会被删除；
    * **类型断言不是类型转换，它不会真的影响到变量的类型**
* 类型断言 vs 类型声明
    * 类型断言只用保证两者间能够相互兼容即可；
    * 类型声明则需要保证待声明的对象的类型必须兼容赋值类型。