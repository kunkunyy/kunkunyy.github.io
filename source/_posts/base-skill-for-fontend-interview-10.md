---
title: 数据类型（十）闭包
date: 2023-10-20 21:45:25
tags:
- 前端面试基本功
categories:
- [前端面试]
- [前端面试题]
---

# 闭包

* 即使外部函数已经不存在，也可以获取作用域链上变量的函数。

```js
function outer(){
  const a = 1;
  function f(){
    console.log(a)
  }
  return f;
}

let f = outer()
f()
```

# 内存泄漏

* 常见于监听DOM元素时，做了一些额外的操作，然后在某些API作用下导致DOM元素不存在后，变量未被清除。

# 面试题

* 一定要在外部函数中 return 内部函数，才会形成闭包吗？
  * 不需要，只要满足能够访问外部变量的函数就是闭包。