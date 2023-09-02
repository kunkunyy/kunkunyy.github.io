---
title: JavaScript学习笔记（四）变量声明关键字
date: 2021-07-21 10:40:51
tags:
- JS学习
categories:
- [前端学习笔记]
- [JS]
- [ES6]
---

# var

## 作用域

* 使用var操作符定义的变量会成为包含它的函数的**局部变量**
* **去掉var操作符，该操作符成为全局变量**
* 严格模式下，如果向这样给未声明的变量赋值，会导致抛出 ReferenceError。
```js
function test(){
    var message = "Hello World!";
}
test()
console.log(message)//报错
----------------------
function test(){
    message = "Hello World!";
}
test()
console.log(message)//Hello World!
```

## 声明提升

* 把所有变量声明都拉到函数作用域的顶部

```js
function test(){
    console.log(message);
    var message = "Hello World!";
}
test()//underfined
--------等价如下代码--------
function test(){
    var message;
    console.log(message);
    message = "Hello World!";
}
test()//underfined
```

# let

* **块作用域：作用域仅限于该块内部。**
~~~js
if(true){
    let message = "Hello World!";
    console.log(message)//Hello World!
}
console.log(message)//ReferenceError:message没有定义
~~~

* **同一个块级作用域不能出现重复声明**
* 声明冗余报错不会因混用 let 和 var 而受影响
    * 原因：这两个关键字声明的并不是不同类型的变量，它们只是指出变量在相关作用域如何存在
~~~js
var message;
var message;
//正常声明不会报错
--------------
let message;
let message;
//SyntaxError:标识符message已经声明过了
--------------
let message;
var message;
//SyntaxError:标识符message已经声明过了
~~~

* 对于嵌套使用相同标识符，不会出错。
~~~js
//声明的变量的值被覆盖
if(true){
    var message = "Hello";
    console.log(message)//Hello
    if(true){
        var message = "World!";
        console.log(message)//World!
    }
}
-----------------------------------
//声明变量的作用域不同
if(true){
    let message = "Hello";
    console.log(message)//Hello
    if(true){
        let message = "World!";
        console.log(message)//World!
    }
}
~~~

## 暂时性死区

* **定义：let声明之前的执行瞬间被称为“暂时性死区”**
    * 在此阶段引用任何后面才声明的变量都会抛出 ReferenceError。
~~~js
console.log(message);//ReferenceError:message没有定义
let message = "Hello World!";
~~~

## 全局声明

* let在全局作用域中声明的变量，**不会成为windows对象的属性**
~~~js
var message = "Hello World!";
console.log(window.message)//Hello World!
-------------------
let message = "Hello World!";
console.log(window.message)//underfined
~~~

## 条件声明

* let不能依赖条件声明模式

## for循环中的let声明

* 对比以下实例：

~~~js
for(var i=0;i<5;i++){

}
console.log(i)//5
------------------
for(let i=0;i<5;i++){

}
console.log(i)//ReferenceError:i未定义
~~~
* var声明的迭代变量保存的是导致循环退出的值
* let声明的迭代变量会为**每一个迭代循环声明一个新的迭代变量**
~~~js
for(var i=0;i<5;i++){
    setTimeout(()=>console.log(i),0)
}//5,5,5,5,5
--------------------
for(let i=0;i<5;i++){
    setTimeout(()=>console.log(i),0)
}//0,1,2,3,4
~~~

# const声明

* 与let基本相同
* 声明变量时必须同时初始化变量，且初始化后不能被修改
* const声明的限制只适用于它指向的变量的引用
~~~js
const message = {};
message.first = "Hello World!";
message.second = "Say GoodBye!";
~~~
* 对于for-of和for-in循环可以很好的使用
~~~js
for(const key in {a:1,b:2}){
    console.log(key)
}//a,b
~~~

# 总结比较

| 比较项 | var | let | const |
| :----: | :----: | :----: | :----: |
| 作用域 | 全局作用域 | 块级作用域 | 块级作用域 |
| 变量声明提升 | 是 | 否 | 否 |
| 重复声明 | 是 | 否 | 否 |
| 优先级 | 尽量不使用 | 次之 | 最高 |