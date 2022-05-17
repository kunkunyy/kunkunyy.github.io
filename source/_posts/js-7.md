---
title: JavaScript学习笔记（七）语句
date: 2021-07-21 18:42:10
tags:
- JS学习
categories:
- [前端学习笔记]
- [JS]
---

# if语句

* 语法格式：
    * **if(condition) statement1 else statement2**
~~~js
if(condition1){
    statement1
} else if(condition2){
    statement2
} else{
    statement3
}
~~~

# do-while语句

* 后循环语句：循环体中的代码执行后才会对推出条件进行求值
* 语法格式：

~~~js
do{
    statement
}while (expression);
~~~

# while语句

* 语法格式：

~~~js
while(expression){
    statement
}
~~~

# for语句

* 语法格式：

~~~js
for(initialization;expression;post-loop-expression){
    statement
}
~~~

* 无穷循环：

~~~js
for(::){
    statement
}
~~~

# for-in语句

* for-in 语句是一种严格的迭代语句，用于枚举对象中的非符号键属性
* 语法格式：

~~~js
for(property in expression){
    statement
}
~~~
* 所有可枚举的属性都会返回一次，但返回的顺序可能会因浏览器而异
* 如果 for-in 循环要迭代的变量是 null 或 undefined，则不执行循环体

# for-of语句

* for-of 语句是一种严格的迭代语句，用于遍历可迭代对象的元素
* 语法格式：

~~~js
for(property of expression){
    statement
}
~~~

* for-of 循环会按照可迭代对象的 next()方法产生值的顺序迭代元素
* 如果尝试迭代的变量不支持迭代，则 for-of 语句会抛出错误
* ES2018 对 for-of 语句进行了扩展，增加了 for-await-of 循环支持生成期约的异步可迭代对象。

# 标签语句

* 语法格式：
~~~js
label: statement
//--------举例---------
start: for(let i=0;i<count;i++){
    console.log(i);
}
~~~

# break和continue语句

* break 语句用于立即退出循环，强制执行循环后的下一条语句
* continue 语句也用于立即退出循环，但会再次从循环顶部开始执行

# with语句

* with 语句的用途是将代码作用域设置为特定的对象
* 语法格式：
~~~js
with (expression) statement;
//---------举例----------
let a = location.search.sustring(1);
let b = location.hostname;
let c = location.href;
//等价于
with(location){
    let a = search.sustring(1);
    let b = hostname;
    let c = href;
}
~~~

# switch语句

* 语法格式：
~~~js
switch(expression){
    case value1:
        statement
        break;
    case value2:
        statement
        break;
    case value3:
        statement
        break;
    default:
        statement
}
~~~
* switch 语句可以用于所有数据类型,因此可以使用字符串甚至对象
* 条件的值不需要是常量，也可以是变量或表达式
