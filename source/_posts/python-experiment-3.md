---
title: Python 实验三 程序的控制结构（分支）
date: 2021-04-05 17:28:55
tags:
- python-experiment
- experiment
categories:
- experiment
---

# 实验目的
1、掌握程序的单分支结构
2、掌握程序的双分支结构
3、掌握程序的多分支结构
4、掌握 if 的嵌套
# 实验内容
## 练习一
### 题目：身体质量指数 BMI
BMI 值可以“客观的”衡量个人的肥胖程度或者说健康程度。世界卫生组织（WHO）根据对全球人口体重的统计认为，BMI 值低于 18.5 kg/m2 时属于“过轻”，表明个体可能营养不良或者饮食无法保障；BMI 值高于 25 kg/m2 时属于“过重”。根据下表所示指标编程测试自己的身体指数状况。
￼![在这里插入图片描述](https://img-blog.csdnimg.cn/20210112004903680.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzk0Nzk0Mw==,size_16,color_FFFFFF,t_70)
### 代码
```python
height ,weight = eval(input("请输入身高（米）和体重（公斤）[逗号隔开]："))
bmi = weight / pow(height,2)
print("BMI 指数为：{:.2f}".format(bmi))
who,dom="",""
if bmi < 18.5:
	who,dom = "偏瘦","偏瘦"
elif 18.5<= bmi < 24:
	who, dom = "正常", "正常"
elif 24<= bmi < 25:
	who, dom = "正常", "偏胖"
elif 25<= bmi < 28:
	who, dom = "偏胖", "偏胖"
elif 28<= bmi < 30:
	who, dom = "偏胖", "肥胖"
else:
	who, dom = "肥胖", "肥胖"
print("BMI 指标为：国际'{0}'，国内'{1}'".format(who,dom))
```
## 练习二
### 题目：学生成绩等级判断
编程实现输入学生成绩 score，得出其等级状况 grade,其对应关系如下：
100>=Score>=85 grade=”A”
70<=Score<85 grade=”B”
60<=Score<70 grade=”C”
0<Score<60 grade=”D”
Score>100 或 Score<0 给出出错提示
### 代码
```python
score = input("请输入你的成绩：")
try:
	score = eval(score)
	if score < 0 or score > 100: 5. print("成绩输入有误，请重新输入")
	else:
		if 0 <= score < 60:
			grade = "D"
		elif 60 <= score < 70:
			grade = "C"
		elif 70 <= score < 85:
			grade = "B"
		elif 85 <= score <= 100:
			grade = "A" 15. print("你的成绩属于{}级别".format(grade))
except NameError:
	print("输入错误，请输入一个整数！")
```
## 练习三
### 题目：猜数游戏
在程序中预设一个 0-9 之间的整数，让用户通过键盘输入所猜的数，如果大于预设的数，显示“遗憾，太大了”；小于预设的数，显示“遗憾，太小了”，如此循环，直到猜中该数，显示“预测 N 次，你猜中了！”，其中 N 是用户输入的数字次数。
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

