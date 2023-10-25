---
title: 数据类型（八）This指向
date: 2023-10-20 16:22:51
tags:
---

# this的绑定时机

* this和函数定义的位置没有关系，只和调用者有关
* this是在运行时被绑定的

```js
function foo{
  console.log(this)
}
foo() // window

let obj = {
  foo: foo
}
obj.foo() // obj

foo.call('hello')// string('hello')
```

# this的绑定规则

* 隐式绑定
  * 通过对象调用函数绑定this
* 显示绑定：
  * call、bind
* 使用new关键字
  * 如果返回的是对象，则直接返回该对象，
  * 如果返回的是基本类型，则return语句无效，仍然返回我们创建的新对象

# 多重绑定优先级

* new绑定 > 显示绑定(bind) > 隐式绑定

# 面试题

```js
// 输出结果：obj1
// 谁直接调用this就指向谁
function foo{
  console.log(this)
}

let obj1 = {
  name: 'obj1',
  foo: foo
}

let obj2 = {
  name:'obj2',
  obj1: obj1
}
obj2.obj1.foo()
```

```
通过new关键字创建一个新对象的步骤是什么/构造函数是如何创建新对象的？
1、创建一个空对象
2、空对象的 __proto__ 指向构造函数的prototype属性
3、执行构造函数，如果构造函数中有this，则将this指向创建的空对象
4、返回刚刚创建的空对象
```

```js
// 输出结果
function foo{
  console.log(this)
}

let obj1 = {
  name: 'obj1',
  foo: foo
}

let obj2 = {
  name:'obj2',
}
(obj2.foo = obj1.foo)()// 等价于foo()
// window
```

```js
// 输出结果
let name = '全局window'
let person = {
  name: "person",
  sayName: function(){
    console.log(this.name)
  }
}
function sayName(){
  let fun = person.sayName;
  fun();
  person.sayName();
  (b = person.sayName)()
}
sayName()
// 全局window、person、全局window
```