---
title: Python 实验六
date: 2021-04-05 17:37:30
tags:
- python-experiment
- experiment
categories:
- experiment
---

# 实验目的
1、 培养分析问题并对进行建模的能力。
2、 熟练使用组合数据类型解决实际问题。
3、 熟练运用选择结构和循环结构解决实际问题。
# 实验内容
## 练习一
### 题目
统计《三国演义》中人物出场次数最多的前 20 人。
### 代码
```python
import jieba as jie

text = (open('三国演义.txt','r',encoding='utf-8')).read()
words = jie.lcut(text)
nowords ={"这个","引兵","次日","人马","不知","汉中","众将",
"只见","大喜","天下","东吴","于是","今日","不敢","魏兵",
"陛下","太守","天子","一面","原来","令人","江东","喊声",
"下马","何不","大军","忽报","先生","百姓","然后","何故",
"先锋","不如","赶来","此人","夫人","先主","后人","背后",
"城中","蜀兵","上马","大叫","都督","一人","如何","商议",
"却说","不可","不能","如此","将军","二人","后主","荆州",
"如何","主公","军马","军士","左右","正是","徐州","忽然",
"因此","成都","未知","不见","大败","大事","之后","一军",
"起兵","引军","军中","接应","进兵","大惊","可以","大怒",
"不得","以为","心中","一声","下文","曹兵","追赶"}
counts ={}
for word in words:
	if len(word) == 1:
		continue
	elif word == "诸葛亮" or word == "孔明曰":
		rword = "孔明"
	elif word == "玄德" or word == "玄德曰":
		rword = "刘备"
	elif word == "孟德" or word == "丞相":
		rword = "曹操"
	elif word == "关公" or word == "云长":
		rword = "关羽"
	else:
		rword = word
	counts[rword] = counts.get(rword,0) + 1
for word in nowords:
	del(counts[word])
items = list(counts.items())
items.sort(key=lambda x:x[1],reverse = True)
for i in range(20):
	word,count = items[i]
	print("{0:<10}{1:>5}".format(word,count))
```
## 练习二
### 题目
编写程序，模拟抓狐狸小游戏。假设一共有一排 5 个洞口，小狐狸最开始的时候在其中一个洞口，然后玩家随机打开一个洞口，如果里面有狐狸就抓到了。如果洞口里没有狐狸就第二天再来抓， 但是第二天狐狸会在玩家来抓之前跳到隔壁洞口里。
### 代码
```python
import random as ran

flag =ran.randint(0,4)
tiao = [-1,1]
while True:
	try:
		inp = eval(input("请输入 0-4 中任意一个数："))
	except:
		print("输入格式有误，请重新输入！")
		continue
	if inp <0 or inp >4:
		print("输入范围有误！")
		continue
	if inp == flag:
		print("找到了，游戏结束！")
		break
	else:
		flag += tiao[ran.randint(0,1)]
		flag %= 5
		print(flag)
```
