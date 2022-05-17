---
title: Python 实验九 文件与数据格式化
date: 2021-04-05 17:47:20
tags:
- python-experiment
- experiment
categories:
- experiment
---

# 实验目的
1. 掌握文件的基本操作
2. 理解一、二维和高维数据的格式化过程
3. 掌握 csv 和 json 格式的相互转换
4. 综合应用组合数据类型与 CSV 和 JSON 数据格式编写简单的应用程序
# 实验内容
## 练习一
### 题目
将提供的 test.csv 文件，具体内容如下：
￼![在这里插入图片描述](https://img-blog.csdnimg.cn/20210112024827802.png)
编程读入该文件，转换成 JSON 格式文件，并以文件名 out.json 输出。转换
后的结果如下所示：
[
	{
 		"同比": "120.7",
 		"城市": "北京",
 		"定基": "121.4",
 		"环比": "101.5"
 	},
 	{
 		"同比": "127.3",
 		"城市": "上海",
 		"定基": "127.8",
 		"环比": "101.2"
 	}
	….
]
### 代码
```python
#方法一：
import json
fr = open("test.csv","r")
ls = []
for line in fr:
    line = line.replace("\n","")
    ls.append(line.split(','))
fr.close()

fw = open("test.json","w")
for i in range(1,len(ls)):
    ls[i]= dict(zip(ls[0],ls[i]))
json.dump(ls[1:],fw,sort_keys=True,indent=4,ensure_ascii=False)
fw.close()
#方法二：
import json
with open("test.csv","r") as fr:
    ls = []
    for line in fr:
        line = line.replace("\n", "")
        ls.append(line.split(','))
    fr.close()
fw = open("test.json","w")
for i in range(1,len(ls)):
    ls[i]= dict(zip(ls[0],ls[i]))
json.dump(ls[1:],fw,sort_keys=True,indent=4,ensure_ascii=False)
fw.close()
```
## 练习二
### 题目
编写程序制作英文学习字典，词典基本功能如下：
(1) 程序读取源文件路径下的 txt 格式词典文件，若没有就创建一个。 词典文件存储方式为 “英文单词 中文单词”,每行仅有一对中英文释义；
(2) 程序有添加功能，输入英文单词，如果没有可以添加中文释义，如果有就显示”已经存在，不能添加”；
(3) 程序有查询功能，如果存在，则显示其中文释义，不存在就显示不存在；
(4) 程序有正常退出的操作。
### 代码
```python
import os
keys = []
dic = {}

def readdict():
    if os.path.exists("dictionary.txt"):
        fr = open('dictionary.txt','r')
    else:
        fr = open('dictionary.txt','w+')
    for line in fr:
        line = line.replace("\n","")
        v = line.split(':')
        dic[v[0]]= v[1]
        keys.append(v[0])
    fr.close()

def writedict(key,value):
    with open('dictionary.txt','a')as fw:
        fw.write(key+':'+value+'\n')

def mydict():
    n = input("请输入进入相应模块（添加，查询，退出）")
    if n == "添加":
        key = input("请输入英文单词：")
        if key not in keys:
            value =input("字典中未找到单词'{}'的中文释义，请输入该单词的中文意思，添加进字典中！".format(key))
            dic[key] = value
            keys.append(key)
            writedict(key,value)
        else:
            print("单词'{}'已存在，不能添加".format(key))
        return 0
    elif n == "查询":
        key = input("请输入英文单词:")
        if key not in keys:
            print("英文单词'{}'不在字典内".format(key))
        else:
            print(dic[key])
        return 0
    elif n == "退出":
        return 1
    else:
        print("输入有误")
        return 0

def main():
    readdict()
    while True:
        n = mydict()
        if n == 1:
            break
        if n == 0:
            continue
main()
```
