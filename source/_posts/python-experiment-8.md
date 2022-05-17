---
title: Python 实验八 函数
date: 2021-04-05 17:45:35
tags:
- python-experiment
- experiment
categories:
- experiment
---

# 实验目的
1. 掌握函数的定义与调用过程
2. 掌握参数的传递方式和传递过程
3. 理解和使用匿名函数
4. 理解递归调用的思想和方法
5. 掌握变量的作用域
# 实验内容
## 练习一
### 题目
定义求 n!的函数 fact()和求和函数 sum(),在此基础上编程实现 1！+2！+…m!的计算。
### 代码
```python
def fact(num):
    result = 1
    for i in range(1,num+1):
        result *= i
    return result
def sum(num):
    result = 0
    for i in range(1,num+1):
        result = result + fact(i)
    return result
num=eval(input("请输入一个数m："))
print("1!+2!+...+{}!={}".format(num,sum(num)))
```
## 练习二
### 题目
定义匿名函数实现求平方，定义判素数函数 list_prime(),该函数可以实现接受任意个数的判断，并将所有素数作为返回值。在此基础上编程实现随机输入任意个数，从中挑选出所有素数，并计算所有素数平方和。
### 代码
```python
def list_prime(num):
    sum = 0
    for i in range(1,num):
        if (num % i==0):
            sum +=1;
    if sum>=2:
        return 0
    else:
        return pingfang(num)

def pingfang(num):
    return num*num;

str = eval(input("请随机输入任意个数(以逗号分割)："))
sum = 0
for i in str:
    sum += list_prime(i)
print("你输入的数组中所有素数的平方和为：{}".format(sum))
```
## 练习三
### 题目
分别定义 numlist()和 charlist()函数，numlist()功能是生成由数字 1-26 构成的列表,charlist()功能是生成由字符 A-Z 构成的列表。在此基础上编写程序实现
生成一个字典，具体如下：
{1: 'a', 2: 'b', 3: 'c', 4: 'd', 5: 'e', 6: 'f', 7: 'g', 8: 'h', 9: 'i', 10: 'j', 11: 'k', 12: 'l', 13: 'm',14: 'n', 15: 'o', 16: 'p', 17: 'q', 18: 'r', 19: 's', 20: 't', 21: 'u', 22: 'v', 23: 'w', 24: 'x', 25: 'y',26: 'z'}
遍历字典，输出所有键值为偶数的元素。
### 代码
```python
def numlist():
    num= []
    for i in range(1,27):
        num.append(i)
    return num
def charlist():
    char = []
    for i in range(26):
        char.append(chr(ord('A')+i))
    return char
dictionary = dict(zip(numlist(),charlist()))
# print(dictionary)
for key in dictionary:
    if key%2 ==0:
        print(dictionary[key])
```
## 练习四
### 题目
绘制科赫雪花，效果如下：
￼![在这里插入图片描述](https://img-blog.csdnimg.cn/20210112024337701.png)
### 代码
```python
import turtle
def koch(size,n):
    if n==0:
        turtle.fd(size)
    else:
        for angle in [0,60,-120,60]:
            turtle.left(angle)
            koch(size/3,n-1)
def main():
    turtle.setup(600,600)
    turtle.speed(0)
    turtle.penup()
    turtle.goto(-200,100)
    turtle.pendown()
    turtle.pensize(2)
    level = 5
    koch(400,level)
    turtle.right(120)
    koch(400,level)
    turtle.right(120)
    koch(400,level)
    turtle.hideturtle()
    turtle.exitonclick()
main()
```
