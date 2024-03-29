---
title: Python max函数不同参数结果比较
date: 2021-03-15 22:29:08
categories:
- python
tags:
- Skill
- python
---

# 前言

&emsp; &emsp; 在菜鸟教程中对于max函数描述是这样的：max() 方法返回给定参数的最大值，参数可以为序列。这个描述只是客观的将这个函数的功能给描述出来了，但这只是一个模糊的定义。由于没有指定可传入参数的类型，所以说这就带给了这个函数无限种可能。现在就来讨论讨论吧！如果有哪些地方表述不是很正确希望大家在评论区指出来哦！

## 数字类型

&emsp; &emsp; 首先我们来看看最简单大数字类型。毋庸置疑，当参数是数字的时候，直接返回最大的数就好了。但是要注意的是**这里的数字指的是一系列数，返回其中最大的。**
下面来看一个简单的例子。
```python
>>> max(1,2,3,4,5,6,7,8,9)
9
```

## 字符串类型

&emsp; &emsp; 我们知道python中字符串是由单引号双引号括起来的一个字符序列。那么当输入max的变量为一个字符串它会返回给我们什么结果呢？

&emsp; &emsp; 我们先看下面这个例子：
```python
>>>a='1,6,0,4,5'
>>>max(a)
'6'
```
我们输入一个由单个数字组成的字符串，穿入max函数后返回了其中的 '6' 这个字符。难道是将字符转化为数字然后再比较大小吗？那字符串中的 ',' 又去哪了呢？那如果是字母又如何进行比较呢？来看下一个例子：
```python
>>>b='fabced'
>>>max(b)
'f'
```
这里变量b为一个字母组成的字符串，结果返回给我们的是字符 'f' ，这里我们大致应该可以判断出来：**当max函数的输入变量为一个字符串的时候，则返回字符串中字符所对应的ASCII码最大的那个字符。**

&emsp; &emsp; 为了验证这一结论我们利用下面这个例子进行验证：
```python
>>>c='1,a,2,c'
>>>max(c)
'c'
```
&emsp; &emsp; 从这个例子我们就可以验证刚刚的结论了：c这个字符串中字符'1''a''2''c'对应的ASCII码的大小分别为49，97，50，99，所以字符'c'所对应的ASCII码值最大固返回字符'c'。那么字符','去哪了呢？实际上字符','也是参与了比较，由于它所对应的ASCII码为44最小，所以看似跟舍弃了一样。
```python
>>>c='1,a,2,c'
>>>min(c)
','
```

另外，max函数还可以对于传入的可迭代对象找出元素中的最大值。这里可迭代的对象主要是列表和字典

## 列表

同样对于列表，它满足可迭代这样一个特点，这里我们主要分四种情况来讨论：

* 元素全为数字
```python
>>>a=[1,2,3,4,5]
>>>max(a)
5
```
* 元素全为字符
```python
>>>b=['a','b','c','d','e']
>>>max(b)
'e'
```
```python
>>>c=['ab','ac','ad','ae']
>>>max(c)
'ae'
```
```python
>>>d=['1a','1b','2a','2b']
>>>max(d)
'2b'
```
通过以上几个例子我们不难发现：
* **列表中元素类型需一致；（前提条件）**
* **若列表中元素数字类型值，则返回列表中元素的最大值；**
* **若列表中元素为字符，字符转换为对应ASCII码值输出最大的；**
* **若列表中元素为字符串，将组成字符串的每个字符的ASCII码相加，然后输出最大的。**

## 列表（元组）

刚刚我们讨论了简单元素组成的列表作为参数的情况，接下来我们看一看由元组组成的列表：我们还是按照上面的步骤来。

* 全为数字
```python
>>>a=[(1,3),(2,2),(1,4),(3,0)]
>>>max(a)
(3,0)
```
* 对应位置元素类型相同
```python
>>>b=[('a',1),('b',5),('a',6)]
>>>max(b)
('b',5)
```
```python
>>>c=[('a','A'),('a','B')]
>>>max(c)
('a','B')
```
* 元组大小不一致
```python
>>>d=[(1,4),(3,1,5),(3,1)]
>>>max(d)
(3,1,5)
```
* 对应位置元素不同
```python
>>>e=[(6,4),(6,1,5),(3,'a')]
>>>max(e)
(6,4)
```
```python
>>>f=[(1,4),('a',2),(3,1)]
>>>max(e)
TypeError: '>' not supported between instances of 'str' and 'int'
```
通过以上几个例子对比分析我们不难发现：

* 按照元组内部排列顺序**从前到后对应位置**进行比较，**如果对应位置元素类型和大小相同，则比较下一个位置的元素**，按照此规则进行比较直至**找到最大值或者出错**。
* 类型不匹配：列表元组中当比较到**对应位置**发现存在相比较的两个元素**类型不相同**时报错；
* 当比较位置**不存在元素**时，默认为NUL；
* **当比较的元素为数字类型值，则返回列表中元素的最大值；**
* **当比较的元素为字符，字符转换为对应ASCII码值返回；**
* **当比较的元素为字符串，将组成字符串的每个字符的ASCII码相加返回。**

## 字典

* 对于字典类型来说相比较容易一些，只需要比较字典的键值，输出最大的键值；
* 键值的比较同上。

```python
>>>a={1:1,2:19,3:100,4:1}
>>>max(a)
4
>>>b={20:21,5:20,13:14}
>>>max(b)
20
```