---
title: JavaScript学习笔记（八）原始值与引用值
date: 2021-07-21 18:42:57
tags:
- JS学习
categories:
- [前端学习笔记]
- [JS]
---

# 什么是原始值和引用值

* **原始值（primitive value）就是最简单的数据**
    * 原始值有六种：Undefined、Null、Boolean、Number、String 和 Symbol；
    * **保存原始值的变量是按值（by value）访问的**，直接对存储在变量中的实际值进行操作
* **引用值（reference value）则是由多个值构成的对象**
    * 引用值是保存在内存中的对象
    * JavaScript 不允许直接访问内存位置，因此也就不能直接操作对象所在的内存空间
    * **保存引用值的变量是按引用（by reference）访问的**
    * 在操作对象时，实际上操作的是对该对象的引用（reference）而非实际的对象本身

# 动态属性

* 动态属性实际上就是在对象被创建后给这个对象添加新的属性，该属性可以被访问使用，直至对象被销毁或者属性被显示的删除。
* **原始值是不能有属性的，只有引用值可以动态添加后面可以使用的属性**
* 原始类型的初始化可以只使用原始字面量形式。如果使用的是 new 关键字，则 JavaScript 会创建一个 Object 类型的实例，但其行为类似原始值。
~~~js
let str1 = "message";
let str2 = new String("Matt");
str1.age = 18;
str2.age = 20;
console.log(typeof str1);//string
console.log(typeof str2);//onject
console.log(str1.age);//underfined
console.log(str2.age);//20
~~~

# 复制值

## 对于原始值

* 在通过变量把一个原始值赋值到另一个变量时，原始值会被复制到新变量的位置。
* 两个变量可以独立使用，互不干扰。
~~~js
let num1 = 5;
let num2 = num1;
~~~
{% asset_img pic1.png # tu %}

## 对于引用值

* 在把引用值从一个变量赋给另一个变量时，存储在变量中的值也会被复制到新变量所在的位置。区别在于，**这里复制的值实际上是一个指针，它指向存储在堆内存中的对象**。
~~~js
let obj1 = new Object(); 
let obj2 = obj1; 
obj1.name = "Nicholas"; 
console.log(obj2.name); // "Nicholas"
~~~
{% asset_img pic2.png # tu %}

# 传递参数

* **ECMAScript 中所有函数的参数都是按值传递的**
* ECMAScript 中函数的参数就是局部变量
* 对象保存在全局作用域的堆内存上，当对象作为参数传递给函数并在函数内部实现了操作，那么该对象会被修改

~~~js
function addFive(num){
    num += 5;
    return num;
}
let count = 5;
let result = addFive(count);
console.log(count);//5
console.log(result);//10
//----------------------------
function setName(obj){
    obj.name = "Nicholas";
}
let person = new Object();
setName(person);
console.log(person.name);//"Nicholas"
~~~

# 确定类型

* typeof 操作符最适合用来判断一个变量是否为原始类型，typeof 虽然对原始值很有用，但它对引用值的用处不大。
* instanceof可用来判断具体类型