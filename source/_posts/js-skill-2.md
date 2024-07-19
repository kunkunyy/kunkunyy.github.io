---
title: JS 基础技能2
date: 2024-04-17 10:20:08
series: JS 技巧
cover: /img/article/js-skill.webp
tags:
- 知识点总结
categories:
- [前端刷题]
- [JS]
---

# Object.groupBy

`Object.groupBy` 方法可以根据指定的键将数组分组为对象。
**参数**
- `array: Array<T>`: 需要分组的数组
- `key: Function`: 分组的键

**返回**
- `return: Partial<Record<string, T[]>>`: 返回分组后的对象

```javascript
const data = [
  { id: 1, name: 'Alice', age: 30, sex: 'F' },
  { id: 2, name: 'Bob', age: 25, sex: 'M' },
  { id: 3, name: 'Tom', age: 20, sex: 'M' },
  { id: 4, name: 'Jerry', age: 35, sex: 'M' },
];

const result = Object.groupBy(data,(item)=> item.sex);
console.log(result);
// {
//   F: [ { id: 1, name: 'Alice', age: 30, sex: 'F' } ],
//   M: [ { id: 2, name: 'Bob', age: 25, sex: 'M' }, 
//      { id: 3, name: 'Tom', age: 20, sex: 'M'},
//      { id: 4, name: 'Jerry', age: 35, sex: 'M'}
//   ]
// }
```

# at

`at` 方法返回数组中指定索引的元素。如果索引为负数，则从数组末尾开始计算。

```javascript
const data = [1, 2, 3, 4, 5];
console.log(data.at(0)); // 1
console.log(data.at(-1)); // 5
```

# Object.hasOwn

`Object.hasOwn` 方法用于检查对象是否具有指定的属性。

**参数**
- `obj: Object`: 要检查的对象
- `key: string`: 要检查的属性
- `return: boolean`: 如果对象具有指定的属性，则返回 `true`，否则返回 `false`

```javascript
const obj = { name: 'Alice', age: 30 };
console.log(Object.hasOwn(obj, 'name')); // true

```