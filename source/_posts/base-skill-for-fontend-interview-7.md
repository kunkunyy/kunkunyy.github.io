---
title: 数据类型（七）面向对象
date: 2023-10-19 22:45:18
tags:
- 前端面试基本功
categories:
- [前端面试]
- [前端面试题]
---

* 构造函数：任何函数都可以当做构造函数，只要使用 new 关键字调用函数创建实例对象（箭头函数除外）
* 继承：
  * 原型链继承: 缺点如果某个原型上的引用类型被其中一个实例改变了，那么其他实例也会受到影响。
```js
function Parent(){
  this.name = 'this is parent!'
  this.getName = function(){
    return this.name;
  }
}
function Child(){
  this.name = 'this is child!'
}
Child.prototype = new Parent()
console.log(new Child().getName())
```
  * 构造函数继承：通过使用 call 或 apply 方法，实现在子类中执行父类型的构造函数
    * 优点：原型属性不会被共享
    * 缺陷：不会继承父类 prototype 上属性
```js
function Parent(){
  this.sayHello = function(){
    console.log('Hello!')
  }
}
function Child(){
  Parent.call(this)
}
```
  * 组合继承：原型链+构造函数继承
    * 调用了两次 Parent
  * 寄生组合继承
    * Child.prototype 上的原始属性会被丢掉
```js
const Parent = function(){
  this.name = 'this is parent'
  this.getName = function(){
    return this.name
  }
}

const Child = function(){
  Parent.call(this)
  this.age = '18'
  this.getAge = function(){
    return this.age
  }
}

Child.prototype = Object.create(Person.prototype)
Child.prototype.constructor = Child
```