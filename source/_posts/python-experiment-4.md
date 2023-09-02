---
title: Python 实验四 程序的控制结构（循环）
date: 2021-04-05 17:31:53
tags:
- python-experiment
- experiment
categories:
- experiment
---

# 实验目的

1、掌握程序的几种循环结构及扩展用法
2、掌握 break 和 continue 的用法
3、掌握 random 库的用法
4、了解程序的异常处理及用法

# 实验内容
## 练习一
### 题目：统计不同字符个数
用户从键盘输入一行字符，编写一个程序，统计并输出其中英文字符、数字、空格和其它字符的个数。
### 代码
```python
charts = input("请输入一行字符：")
english = num = space = other = 0
for i in charts:
	if '0'<= i <= '9':
		num += 1
	elif ('a' <= i <= 'z') or ('A' <= i <='Z'):
		english += 1
	elif i == ' ':
		space += 1
	else:
		other += 1
print("在你输入的字符串中：英文字符有{}个，数字有{}个，空格有{}个，其他字符有{}个".format(english,num,space,other))
```
## 练习二
### 题目：猜数游戏续
在上一次猜游戏实验题目的基础上，完善程序，实现如下的功能：
系统自动生成 1-100 以内的随机整数，让用户通过键盘输入所猜的数，如果大于预设的数，显示“遗憾，太大了”；小于预设的数，显示“遗憾，太小了”，如此循环，直到猜中该数，显示“预测 N 次，你猜中了！”，其中 N 是用户输入的数字次数。如果用户输入的不是整数，而是小数，则提示用户“输入错误，必须输入整数！”，并让用户重新输入。如果用户输入的不是数字，则给出出错提示“输入格式错误，结束程序！”
### 代码
```python
import random as rand;
flag = rand.randint(0,9)
count = 0
while True:
	num = input("请输入你猜想的数：")
	try: 8. num = eval(num)
		if num < flag :
			print("遗憾，太小了！")
			count += 1
			continue
		elif num > flag:
			print("遗憾，太大了！")
			count += 1
			continue
		elif num == flag:
			count += 1
			print("预测{}次，你猜中了！".format(count))
			st = input("是否继续游戏!\n 输入 1 继续，输入 0 结束：")
			if eval(st)==1:
				count = 0
				flag =rand.randint(0,9)
				continue
			else:
				exit()
	except NameError:
	print("输入类型错误，请输入一个整数，程序执行完毕！")
	exit()
```
## 练习三
### 题目：最大公约数计算
从键盘接收两个整数，编写程序求出这两个整数的最大公约数和最小公倍数（提示：用辗转相除法求最大公约数，用两数乘积除以最大公约数求得最小公倍数）
### 代码
```python
num1,num2 = eval(input("请输入两个整数(逗号隔开输入)："))
greatest_common_divisor = 0
least_common_multiple = 0
def Calculation(a,b):
	if b == 0:
		return a
		return Calculation(b,a%b)
if num1 < num2:
	temp = num1
	num1 = num2
	num2 = temp
greatest_common_divisor = Calculation(num1,num2)
least_common_multiple = num2 * num1 / greatest_common_divisor
print(" 两 数 的 最 大 公 约 数 为 {} ， 最 小 公 倍 数 为{:.0f}".format(greatest_common_divisor,least_common_multiple))
```
## 练习四
### 题目
请编写程序实现如下数字金字塔的显示：
￼![在这里插入图片描述](https://img-blog.csdnimg.cn/202101120129343.png)
### 代码
```python
s = ''
for i in range(1,10):
	s = s + str(i)
	left_part = s[::-1]
	right_part = s[1:]
	strs = left_part + right_part
	print(strs.center(17," "))
```
