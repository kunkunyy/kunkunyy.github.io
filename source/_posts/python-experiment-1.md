---
title: Python 实验一 Python运行环境搭建及使用
date: 2021-04-05 15:51:52
tags:
- python-experiment
- experiment
categories:
- experiment
---

# 实验目的
1. 熟悉 Python 开发环境的使用
2. 熟悉 Python 应用程序的创建与运行
3. 掌握 Python 输入与输出
# 实验内容
## 练习一
### 题目
分别用交互模式和批量模式完成以下代码的练习。
```python
str1=input("请输入一个人的名字：")
str2=input("请输入一个国家的名字：")
print("世界这么大，{}想去{}看看".format(str1,str2))
```
## 练习二
### 题目
整数序列求和：用户输入一个正整数 N,计算从 1 到 N(包含 1 和 N)相加之后的结果。
### 代码
```python
N = input("请输入一个正整数N：")
sum = 0
for i in range (int(N)):
    sum += i + 1
print("1到N相加后的结果为：",sum)
```
## 练习三
### 题目
健康食谱输出：列出 5 种不同的食材，输出它们可能组成的所有菜式名称。
### 代码
```python
array = ['土豆','豇豆','辣椒','五花肉','豆腐']
for i in range(0,5):
    for j in range(0,5):
        if not(i==j):
            print("{}{}".format(array[i],array[j]))
```
## 练习四
### 题目
太阳花的绘制：使用 turtle 库绘制一个太阳花的图形，如下图所示。
￼![在这里插入图片描述](https://img-blog.csdnimg.cn/20210112002138847.png)
#### 代码
```python
from turtle import *
color('red','yellow')
begin_fill()
while True:
    forward(200)
    left(170)
    if abs(pos())<1:
        break
end_fill()
done()
```
