---
title: TypeScript学习笔记（八）函数的类型
date: 2022-07-31 17:35:19
cover: /img/article/winter-piece.webp
tags:
- TypeScript学习
categories:
- [前端学习笔记]
- [TypeScript]
---

# 函数声明

* 需要对函数的输入和输出进行限制。
* 输入多余的（或者少于要求的）参数，是不被允许的

```ts
function sum(x: number, y: number): number {
    return x + y;
}
```

# 函数表达式

* 在 TypeScript 的类型定义中，=> 用来表示函数的定义，左边是输入类型，需要用括号括起来，右边是输出类型。

```ts
let mySum: (x: number, y: number) => number = function (x: number, y: number): number {
    return x + y;
};
```

# 用接口定义函数的形状

* 采用函数表达式|接口定义函数的方式时，对等号左侧进行类型限制，可以保证以后对函数名赋值时保证参数个数、参数类型、返回值类型不变。

```ts
interface SearchFunc {
    (source: string, subString: string): boolean;
}

let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
    return source.search(subString) !== -1;
}
```

# 可选参数

* 用 ? 表示可选的参数。
* 可选参数后面不允许再出现必需参数。

```ts
function buildName(firstName: string, lastName?: string) {
    if (lastName) {
        return firstName + ' ' + lastName;
    } else {
        return firstName;
    }
}
let tomcat = buildName('Tom', 'Cat');
let tom = buildName('Tom');
```

# 参数默认值

* TypeScript 会将添加了默认值的参数识别为可选参数：

```ts
function buildName(firstName: string, lastName: string = 'Cat') {
    return firstName + ' ' + lastName;
}
let tomcat = buildName('Tom', 'Cat');
let tom = buildName('Tom');
```

# 剩余参数

* 使用 ```...rest``` 的方式获取函数中的剩余参数（rest 参数）。

```ts
function push(array, ...items) {
    items.forEach(function(item) {
        array.push(item);
    });
}

let a: any[] = [];
push(a, 1, 2, 3);

function push(array: any[], ...items: any[]) {
    items.forEach(function(item) {
        array.push(item);
    });
}

let a = [];
push(a, 1, 2, 3);
```

# 重载

* 重载允许一个函数接受不同数量或类型的参数时，作出不同的处理。
* 定义多个重名函数来实现重载。

```ts
// 实现一个函数 reverse，输入数字 123 的时候，输出反转的数字 321，输入字符串 'hello' 的时候，输出反转的字符串 'olleh'。

function reverse(x: number): number;
function reverse(x: string): string;
function reverse(x: number | string): number | string | void {
    if (typeof x === 'number') {
        return Number(x.toString().split('').reverse().join(''));
    } else if (typeof x === 'string') {
        return x.split('').reverse().join('');
    }
}
```

