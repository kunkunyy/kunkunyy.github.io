---
title: Python 实验五 组合数据类型
date: 2021-04-05 17:34:20
tags:
- python-experiment
- experiment
categories:
- experiment
---

# 实验目的
1、理解组合数据类型的概念
2、掌握列表、字典与集合的定义和使用方法
3、熟练 random 模块的应用
# 实验内容
## 练习一
### 题目
编写程序，在 26 个字母大小写和 10 个数字组成的列表中随机生成 10个 8 位密码。
### 代码
```python
#设置的密码没有重复的元素
import random
List =[]
#存入字母和数字
for i in range(26):
	List.append(chr(ord('A')+i))
	List.append(chr(ord('a')+i))
for i in range(10):
	List.append(str(i))
#随机生成 10 个 8 位数的密码
for i in range(10):
	password_List = random.sample(List,8) #生成结果仍为列表
	password = "".join(password_List) #将列表转化成字符串
	print(password)
```
## 练习二
### 题目
通过键盘输入系列整数值，输入 0 则表示输入结束，将这些值（不含 0）建立为一个列表，并按从大到小的顺序输出该列表各元素。
### 代码
```python
List = []
num = eval(input("请输入一个整数："))
while num != 0:
	List.append(num)
	num = eval(input("请输入一个整数："))
List.sort(reverse = True)
for i in range(len(List)):
	print(List[i])
```
## 练习三
### 题目
输入一个大于 2 的自然数， 输出小于该数字的所有素数组成的集合。
### 代码
```python
import math

def isprime(n):
	for i in range(2,n):
		if n % i ==0:
			return False
		else:
			return True

num_input = eval(input("请输入一个大于 2 的自然数："))
num = math.ceil(num_input)
prime_set = set()
for i in range(2,num):
	if isprime(i):
		prime_set.add(i)
	print("小于{}的所有素数集合是：{}".format(num_input,prime_set))
```
## 练习四
### 题目
使用字典来创建程序，提示用户输入电话号码，并用英文单词形式显示数字。例如：输入 138 则显示“one three eight”。
### 代码
```python
num = [i for i in range(10)]
word = ["zero","one","two","three","four","five","six","seven","eight","nine"]
phone_list = dict(zip(num,word))
phone_num = input("请输入电话号码：")
result=""
#print(phone_num)
for i in phone_num:
	phone_i = eval(i)
	for key in phone_list:
		if phone_i == key:
			result += phone_list[key] + " "
print(result)
```
