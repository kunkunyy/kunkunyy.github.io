---
title: Python 实验二 字符串及基本数据类型操作
date: 2021-04-05 17:26:04
tags:
- python-experiment
- experiment
categories:
- experiment
---

# 实验目的

1．掌握字符串编码、索引方式
2．掌握字符串的操作
3．掌握字符串格式化
4．掌握基本数据类型的运算操作

# 实验内容
## 练习一
### 题目
完成以下代码练习，熟悉字符串的相关使用。
```python
略
```
## 练习二
### 题目
输出由任意字符串堆积的 10 行等腰三角形。其中，str.center()方法用于字符串两边填充：str.rjust(width[,fillchar])方法用于字符串右填充。
### 代码
```python
width = 2*10-1
for i in range(1,11):
    str = '*'*(2*i-1)
    print(str.center(width,' '))
```
### 运行结果
￼![练习二](https://img-blog.csdnimg.cn/20210112003942446.png)
## 练习三
### 题目
能力值的计算：一年 365 天，以第 1 天的能力值为基数，记为 1.0，当每天好好学习时能力值相比前一天提高 1‰，当没有学习时由于遗忘等原因能力值相比前一天下降 1‰，完成下列能力值的计算：
(1) 每天努力和每天放任，一年下来的能力值分别多少？
(2) 一周 5 个工作日，如果每个工作日都好好学习，在周末放任一下，计算 1 年后的能力值。
### 代码
```python
#（1）
dayfactor = 0.0001
dayup = pow(1+dayfactor,365)
daydown = pow(1-dayfactor,365)
print("每天努力：{:.3f},每天放任：{:.3f}".format(dayup,daydown))

#（2）
dayfactor = 0.0001
dayup = 1.0
for i in range(365):
    if i%7 in [6,0]:
        dayup = dayup*(1-dayfactor)
    else:
        dayup = dayup*(1+dayfactor)
print("1年后的能力值{:.2f}".format(dayup))
```
## 练习四
### 题目
凯撒密码：设想在某些情况下给朋友传递字条信息，但又不希望传递中途被第三方看懂这些信息，因此需要对字条信息进行加密处理。凯撒密码采用了替换算法对信息中的每一个英文字符循环替换为该字符后面第三个字符，对应关系如下：
原文：A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
密文：D E F G H I J K L M N O P Q R S T U V W X Y Z A B C
其它字符保持不变。
编程实现：程序接收用户输入待加密的信息，输出加密后的密文。
#### 代码
```python
enter_code = input("请输入一段信息：")
encode = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
output_code = ""

for c in enter_code:
    if c in enter_code:
        index = encode.index(c)
        index += 3
        index %= len(encode)
        output_code += encode[index]
    else:
        output_code += c
print("加密后的密文为：",output_code)
```
## 练习五
### 题目
文本进度条：参考教材，使用字符串完成进度条的设计。要求：
(1) 显示当前进度的百分比；
(2) 动态刷新显示当前的进度。
#### 代码
```python
import time
scale = 50
print("执行开始".center(scale//2,'-'))
t = time.clock()
for i in range(scale+1):
    a = '*' * i
    b = '.' * (scale-i)
    c = (i/scale) * 100
    t -= time.clock()
    print("\r{:^.3f}%[{}->{}]{:.2f}s".format(c,a,b,-t),end='')
    time.sleep(0.05)
print("\n"+"执行结束".center(scale//2,'-'))
```