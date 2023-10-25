---
title: 数据类型（四）类型转换与隐式类型转换
date: 2023-10-19 17:18:59
tags:
- 前端面试基本功
categories:
- [前端面试]
- [前端面试题]
---

# 显示类型转换面试题

```js
console.log(["1", "2", "3"].map(parseInt))// [1, NaN, NaN]
/*
"1"=> parseInt("1", 0)
"2"=> parseInt("2", 1)
"3"=> parseInt("3", 2)
*/
```
* map((item, index, arr))：当前值，下标，整个数组
* parseInt(string, radix)：当前字符串，进制
  
```js
console.log(parseInt(1/0, 19))// 18
/*
1/0 = Infinity
等价于parseInt("Infinity", 19)
19进制有I，但是没有n，则直接返回i，即18
*/
```
```js
console.log(parseInt(parseInt, 16))// 15
/*
String(parseInt)=> "function parseInt() { [native code] }"
等价于parseInt("function parseInt() { [native code] }", 16)
16进制有f，但是没有u，则直接返回f，即15
*/
```
```js
console.log(parseInt({}, 16))// NaN
/*
String({})=> "[object Object]"
等价于parseInt("[object Object]", 16)
16进制没有[，直接返回NaN
*/
```

# 常见的隐式类型转换

* 比较操作符：两侧值的类型不同时执行
  * **当操作数是对象，另一个操作数是字符串或数字时会首先调用valueOf方法，当valueOf方法返回的不是基本类型时，才会去调用toString方法**
* 四则运算，除加法外，其他运算均会被转换为数字进行运算，遇到NaN，结果均为NaN。
  * number + number = number
  * number + string = string
  * string + number = string
  * object + number = number
  * object + string = string
  * boolean + string = string(含true/false)
  * boolean + number = number
* 条件语句

## 面试题

```js
let a = {
  value: 1;
  valueOf: function(){
    this.value++
  }
  toString: function(){
    this.value++;
  }
}
if(a == 1 && a == 2 && a == 3){
  console.log(true)
}
```