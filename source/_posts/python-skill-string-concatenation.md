---
title: Python 字符串连接方法
date: 2021-03-16 10:41:58
categories:
- python
tags:
- Skill
- python
---

之前在学习Python的时候对于字符串的连接并没有过多的研究，能应付考试就好啦。最近在写博客遇到相关内容去查了查，发现字符串连接的方法还是非常多的，这篇博客就来记录一下方便以后查看。

# 字符串连接方法总结
假设'str1'，'str2'为两个字符串。

## '+' 连接
```python
a = str1
b = str2
print(a+b)

>>>str1str2
```

## 模式串"%s"方法
```python
print（'%s%s' %(str1,str2)）

>>>str1str2
```
## format 方法
```python
print("{}{}".format(str1，str2))
print("{0}{1}".format(str1，str2))
print("{c}{d}".format(c=str1，d=str2))

>>>str1str2
```
## 'f-string'方法
```python
print(f'{str1}{str2}') 

>>>str1str2
```
## join方法
* 列表
```python
print(''.join([str1,str2])) 

>>>str1str2
```
* 字典：只有键和值均为字符串时才可使用。
```python
a={'20':'20','520':'1314'}

print(''.join(a))
print(''.join(a.value（））) 

>>> '20520
>>> '201314'
```
## 通过() 多行拼接
```python
s = (
    'Hello'
    ' '
    'World'
    '!'
)
print(s)

>>> Hello World! 
```
## 通过string模块中的Template对象拼接
* 实现原理：通过Template初始化一个字符串。这些字符串中包含了一个个key。通过调用substitute或safe_subsititute，将key值与方法中传递过来的参数对应上，从而实现在指定的位置导入字符串。
```python
from string import Template
s = Template('${s1} ${s2}!')
print(s.safe_substitute(s1='Hello',s2='World'))

>>> Hello World
```
## 空格自动连接
```python
>>> "Hello" "Nasus"
'HelloNasus'
```