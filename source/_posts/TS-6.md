---
title: TypeScript学习笔记（六）对象的类型——接口
date: 2022-07-31 00:31:57
cover: /img/article/winter-trail.webp
tags:
- TypeScript学习
categories:
- [前端学习笔记]
- [TypeScript]
---

# 定义

* 在 TypeScript 中，我们使用接口（Interfaces）来定义对象的类型。

## 接口

* 接口是对行为的抽象，而具体如何行动需要由类（classes）去实现（implement）。
* TypeScript 中的接口是一个非常灵活的概念，除了可用于对类的一部分行为进行抽象以外，也常用于对「对象的形状（Shape）」进行描述。

# 使用

* 接口一般首字母大写。有的编程语言中会建议接口的名称加上 I 前缀。
* 定义的变量比接口多/少了一些属性是不允许的，即：**赋值的时候，变量的形状必须和接口的形状保持一致**

```ts
interface Person {
    name: string;
    age: number;
}

let tom: Person = {
    name: 'Tom',
    age: 25
};

let jack: Person = {
    name: 'Jack'
};

// index.ts(6,5): error TS2322: Type '{ name: string; }' is not assignable to type 'Person'.
//   Property 'age' is missing in type '{ name: string; }'.
```

# 可选属性

* 用于满足不要完全匹配一个形状，即：**该属性是可以不存在**，但仍然不允许添加未定义的属性。

```ts
interface Person {
    name: string;
    age?: number;
}

let tom: Person = {
    name: 'Tom'
};
```

# 任意属性

* 允许接口有任意的属性。
* **一旦定义了任意属性，那么确定属性和可选属性的类型都必须是它的类型的子集**

```ts
interface Person {
    name: string;
    age?: number;
    [propName: string]: any;
}

let tom: Person = {
    name: 'Tom',
    gender: 'male'
};

// index.ts(3,5): error TS2411: Property 'age' of type 'number' is not assignable to string index type 'string'.
// index.ts(7,5): error TS2322: Type '{ [x: string]: string | number; name: string; age: number; gender: string; }' is not assignable to type 'Person'.
//   Index signatures are incompatible.
//     Type 'string | number' is not assignable to type 'string'.
//       Type 'number' is not assignable to type 'string'.
```

* 一个接口中只能定义一个任意属性。如果接口中有多个类型的属性，则可以在任意属性中使用联合类型：

```ts
interface Person {
    name: string;
    age?: number;
    [propName: string]: string | number;
}

let tom: Person = {
    name: 'Tom',
    age: 25,
    gender: 'male'
}
```

# 只读属性

* 用 readonly 定义只读属性，只读属性只能在初始化的时候被赋值，后续不能被修改，如果初始化时 。
* 只读的约束存在于第一次给对象赋值的时候，而不是第一次给只读属性赋值的时候：

```ts
interface Person {
    readonly id: number;
    name: string;
    age?: number;
    [propName: string]: any;
}

let tom: Person = {
    id: 89757,
    name: 'Tom',
    gender: 'male'
};

tom.id = 9527;

// index.ts(14,5): error TS2540: Cannot assign to 'id' because it is a constant or a read-only property.
```