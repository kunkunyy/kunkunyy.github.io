---
title: 数据类型（一）基本类型与应用类型
date: 2023-10-19 16:29:28
tags:
- 前端面试基本功
categories:
- [前端面试]
- [前端面试题]
---

```ts
function modifyNumber(a: number): void{
  a = 18;
}
function modifyArray(arr: number[]): void{
  arr.push(18)
}
let a = 1, b = [1];
modifyNumber(a);
modifyArray(b);
console.log(a);// 1
console.log(b);// [1, 18]
```

# JavaScript 类型

* 原始类型/基本类型：
  * **值存储在栈中，进行赋值操作时，会开辟新的空间存储**，互不影响。
  * Number、String、Boolean、Null、Undefined、Symbol、Bigint
* 引用类型
  * **值存储在堆中，栈中存储的是对象堆内存的地址**，进行赋值操作时，只要修改其中一个变量引用的对象，其它引用该变量也会受到影响
  * Array、Object、Function、Set、Map、Date、RegExp

## Symbol

* 基本用法：
```ts
let id1 = Symbol('id1')
let id2 = Symbol('id2')
// 用法一Symbol.description
console.log(id1.description) // id1
// Symbol.for
console.log(Symbol.for('id1') === Symbol.for('id1')) // true
// Symbol.keyfor 仅用于Symbol.for创建的Symbol
console.log(Symbol.keyfor(Symbol.for('id1')))// id1
console.log(Symbol.keyfor(Symbol('id1')))// undefined
```
* 应用场景：
  * 解决 id 重复。
  * 隐藏对象属性
    * Reflect.ownKeys() -既能遍历

# 面试题

## 如何判断一个变量是数组还是对象

* Object.prototype.toString.call()
  * 函数: [object Array]
  * 数组: [object Object]
* Array.isArray()

## null 是不是对象类型

* 不是，JS底层设计使用一种称为“标签”的机制来存储不同类型的值。
* 对于对象类型，其标签值二进制表示的低三位都是0
* null值在内存中表示为全0，低三位也是0，所以被错误识别为object

## 0.1 + 0.2等于多少

* 数字在计算机中存储为二进制，且位数有限，在对无限位小数进行截取时，需要保留的最后一位，需要根据下一位的值来判断，下一位为1则需要进1。