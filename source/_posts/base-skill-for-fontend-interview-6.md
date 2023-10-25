---
title: 数据类型（六）深拷贝和浅拷贝
date: 2023-10-19 22:08:28
tags:
- 前端面试基本功
categories:
- [前端面试]
- [前端面试题]
---

# 浅拷贝

* 主要针对对象的属性对应的值也是复杂类型的情况，此时浅拷贝的对象在对属性值为复杂类型的属性进行修改时，也会影响到原始对象，即被拷贝的对象。

## 对象的浅拷贝

* Object.assign()
* 展开运算符
* for...in... + Object.hasOwnProperty()
* Object.keys()

## 数组的浅拷贝

* Array.slice(start, end) --- 左闭右开
* Array.concat() ---使用空数组拼接

# 深拷贝

* JSON.parse(JSON.stringify())
  * 无法拷贝函数、特殊对象：Date、Regex
  * 不会拷贝原型链上的属性
  * 会忽略Symbol和undefined属性值对应的属性

## 手写深拷贝函数

```TS
function deepClone(source: Object, cloneMap = new Map()) {
  if (!(source instanceof Object) || source === null) {
    return source;
  }
  if (cloneMap.has(source)) {
    return cloneMap.get(source);
  }
  const target = Array.isArray(source)
    ? []
    : source instanceof Date
    ? new Date(source)
    : source instanceof RegExp
    ? new RegExp(source.source, source.flags)
    : {};
  cloneMap.set(source, target);
  for (const key in source) {
    if (typeof source[key] === "object" && source[key] !== null) {
      target[key] = deepClone(source[key], cloneMap);
    } else {
      target[key] = source[key];
    }
  }
  const symbols = Object.getOwnPropertySymbols(source);
  for(const symKey of symbols) {
    target[symKey] = deepClone(source[symKey], cloneMap);
  }
  return target;
}

```