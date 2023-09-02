---
title: Python 实验七 阶段测试
date: 2021-04-05 17:44:10
tags:
- python-experiment
- experiment
categories:
- experiment
---

## 题目一
打印输出如下字符图案。
￼![在这里插入图片描述](https://img-blog.csdnimg.cn/20210112020330204.png)
### 代码
```python
width_top = 2*6-1;
width_di = 2*5-1;
for i in range(1,7):
    str = '*'*(2*i-1)
    print(str.center(width_top,' '))
for i in range(5,-1,-1):
    str = '*' * (2 * i - 1)
    print(str.center(width_top,' '))
```
## 题目二
输入一个年份，判断并输出该年份是否为闰年。。
### 代码
```python
date = eval(input("请输入一个年份："))
# print(date)
if (date%100!=0 and date%4==0) or (date%400==0):
    print("{}年是闰年".format(date))
else:
    print("{}年不是闰年".format(date))
```
## 题目三
编写一个程序，输入任意一行字符，并将其中的小写字母转换为大写字母，然后打印输出，注意：非小写字母不转换。
### 代码
```python
str = input("请输入一行字符：")
for i in str:
    if 97<=ord(i)<=122:
        i = chr(ord(i)-32)
    print(i,end='')
```
## 题目四
使用turtle库画一个心形，例如：
￼![在这里插入图片描述](https://img-blog.csdnimg.cn/20210112020940184.png)
### 代码
```python
from turtle import *

color('red','pink')
begin_fill()
left(140)
fd(135)
right(180)

circle(60,-180)

backward(35)
right(100)
forward(35)
circle(-60,180)
fd(135)
end_fill()
exitonclick()
```
## 题目五
编写一个程序来计算输入的一行字符串中的单词频率（单词不区分大小写），并按字母顺序对键进行排序，然后输出所有单词及其出现的次数。 提示：对一个字典counts内的单词按字母顺序排序，代码可以为：sorted_words = sorted(counts.keys())

例如，输入：

New to Python or choosing between Python 2 and Python 3? Read Python 2 or Python 3.

则输出为：
￼![在这里插入图片描述](https://img-blog.csdnimg.cn/20210112021105676.png)
### 代码
```python
str = input("请输入一行字符串：")

word_dic={}
word =""

for i in str:
    if 48<=ord(i)<=57 or 97<=ord(i)<=122:
        word = word+i
    elif 65<=ord(i)<=90:
        i = chr(ord(i)+32)
        word = word+i
    else:
        if word == "":
            continue
        else:
            word_value = {word: 1}
            if (word in word_dic.keys()):
                word_dic[word] += 1
                word = ""
            else:
                word_dic.update(word_value)
                word = ""
sorted_word = sorted(word_dic.items(),key=lambda d:d[0])
# print(sorted_word)
for key,value in sorted_word:
    print("{}:{}".format(key,value))
```