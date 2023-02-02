---
title: Lodash学习笔记
date: 2022-09-28 15:58:20
tags:
  - Lodash
categories:
  - Lodash
  - Array
---

# chunk

- 将数组按照指定区块的长度进行拆分
- 如果 array 无法被分割成全部等长的区块，那么最后剩余的元素将组成一个区块
- 参数：(array, [size=1])

```js
const arr = ["a", "b", 1, false, "", { name: "zhangsan" }];
console.log(_.chunk(arr, 2));
// [[ 'a', 'b' ], [ 1, false ], [ '', { name: 'zhangsan' } ] ]
console.log(_.chunk(arr, 4));
// [[ 'a', 'b', 1, false ], [ '', { name: 'zhangsan' } ] ]
```

# compact

- 创建一个新数组，包含原数组中所有的非假值元素。
- ```false```, ```null```,```0```, ```""```, ```undefined``` 和 ```NaN``` 都是被认为是“假值”。

```js
const arr = ["a", "b", 1, false, "", { name: "zhangsan" }];
console.log(_.compact(arr));
// [ 'a', 'b', 1, { name: 'zhangsan' } ]
```

# concat

- 创建一个新数组，将 array 与任何数组 或 值连接在一起。

```js
const arr = ['a','b',1,false,'',{'name':'zhangsan'}]
console.log(_.concat(arr,'abc',['efg'],[['fij']]))
// ['a','b',1,false,'',{ name: 'zhangsan' },'abc','efg',[ 'fij' ]]
```

# difference

- 两数组进行比较，排除待检查数组与目标数组交集取反。

```js
console.log(_.difference([3,2,1],[4,2]))
// [3,1]
```

# isEqual

- 执行深比较来确定两者的值是否相等。

```js
console.log(_.isEqual({'name':'zhangsan'},{'name':'zhangsan'}))
// true
```