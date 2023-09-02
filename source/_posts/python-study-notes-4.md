---
title: Python学习笔记（四）
date: 2021-03-27 17:20:38
categories:
- python
tags:
- study-notes
- python
---

# 函数的使用

## 函数定义

在Python中我们时常会使用函数来提高代码的效率和复用性，函数的定义如下：
```python
def <函数名>(<参数>):
    <函数体>
    return <返回值>
```
* 这里def保留字用于声明函数，必须填写。

## 参数设置

* 参数设置主要有四类：必选参数、默认参数、可选参数、关键字参数

```python
def <函数名>(<必选参数>,<默认参数><可选参数><关键字参数>):
    <函数体>
    return <返回值>
```
下面以计算n!这个函数为例子来介绍这些参数：

### 必选参数传递

* 当函数只有一个参数时默认该参数为必选参数

### 默认参数传递

* 优势：减低函数的难度
* tips：默认参数必须指向不变对象
* 下面例子当中的参数m就是可选参数，
```python
def fact(n,m=1):
    s = 1
    for i in range(1,n+1):
        s *= i
    return s//m
```

### 可变参数传递

* 传入参数的个数是可变的。
* 在参数前面加上*就是可变参数。
* 在函数内部，参数numbers接收得到的是一个tuple，调用该函数时，可以传入任意个参数，包括0个参数
```python
def num(*fail):
    total = 0
    for i in fail:
        total += i
    return total
```
这里fail可以接受多个参数的传递，如下所示：
```python
print(num(1,2))
3
print(num(4,5,6))
15
```

### 关键字参数传递

* 使用**表示关键字参数
```python
def action(time,person,**job):
    print("At",time,person,"wants to",job)

action('7:00','Mom',job = 'clean house')

At 7:00 Mom wants to {'job': 'clean house'}
```
