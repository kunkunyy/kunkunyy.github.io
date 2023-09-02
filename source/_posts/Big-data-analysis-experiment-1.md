---
title: 大数据分析实验（一）
date: 2021-04-01 21:18:27
tags:
- big data analysis experiment
- experiment
categories:
- experiment
---

# Python 快速开发

## 实验目的和要求

* 熟悉 Python 开发环境；
* 掌握项目和文件的建立；
* 掌握 Python 的基本语法；

## 实验内容和分析

### 题目一：自定义函数，设置固定次数的登陆

#### 题目内容

* 由键盘输入密码；
* 若密码正确则屏幕显示：“Login success!”
* 若密码错误则显示：“Wrong password or invalid input”，并显示剩余输入机会次数；
* 共 3 次机会，用完则屏幕显示：“Your account has been suspended”，并退出程序。

#### 解题思路

① 首先创建一个文件模拟数据库用于存储用户的账户和密码
② 通过读取用户的输入来和文件中的账户密码进行比较
③ 如果存在则显示登陆成功，否则显示登陆失败
④ 定义一个变量用于存储用户失败次数，当失败次数大于3后，显示账户已被锁定然后结束程序。

#### 实验设计

① 定义一个函数其参数为用户输入的用户名和密码，该函数将数据库文件打开然后按照存储形式进行比较，如果存在就返回True否则返回False
② 主函数定义变量frequency用于标记用户登陆失败次数
③ 利用while语句实现失败重新登陆这一过程；当匹配成功跳出循环，当超过三次也跳出循环结束程序。

#### 考察知识

* 函数定义
* 文件操作
* while语句

#### 实验代码

```python
#定义账号密码文件
#读取文件，验证用户输入的账号密码是否在文件内
#存在返回true，不存在返回false
def Login_authentication(user, pwd):
    f = open("information.txt", mode="r", encoding="UTF-8")
    for line in f:
        #按照你在文件中对字符串的存储方式来进行字符串比较
        if line.strip() == user + "-" + pwd:
            f.close()
            return True
    else:
        f.close()
        return False
#存储错误次数
frequency = 0
while frequency < 3:
    #读取用户输入调用验证函数
    user_name = input("Please enter user name：")
    user_password = input("Please enter user password:")
    ret = Login_authentication(user_name,user_password)
    if ret:
        print("Login success!")
        break
    else:
        frequency += 1
        if frequency == 3:
            print("Your account has been suspended")
        else:
            print("Wrong password or invalid input,remaining times:{}".format(3-frequency))
            print(end="\n")
```

#### 实验结果

* 运行结果：
{% asset_img topic1.png # 结果 %}
* 数据库文件：
{% asset_img infor.png # 数据库 %}

### 题目二：自定义函数, 将敏感词过滤后的文本写入指定位置的 txt

#### 题目内容

* 自定义函数 texcreat(name,text),将 text 写入“name.txt”文件中
* 自定义函数 Repalce,将某些敏感词(比如“暴力”)替换成“**”
* 自定义函数将敏感词过滤后的文本写入指定 txt 文档中。

#### 解题思路

① 创建一个文件用于存储定义敏感词；
② 将用户输入的一句话作为参数传到敏感词过滤函数当中；
③ 遍历该函数通过敏感词读取函数返回的由敏感词组成的列表比较该句话中是否存在敏感词，如果存在则将对应敏感词的字符长度存储下来然后用*来替代；
④ 最后再将过滤完成的句子存放到以用户输入的文件名而创建的文件当中。

#### 实验设计

① 创建一个存放敏感词的文件sensitive_word，输入一些敏感词存入到里面；
② 定义一个函数read_sensitive_word函数用于读取文件中的敏感词然后将其存放在列表中，将存储敏感词的列表当作值进行返回；
③ 创建一个函数texcreat用于接收用户输入的文件名然后创建这个文件，并将过滤敏感词后的句子存到该文件中；
④ 定义一个函数Replace用于过滤敏感词；
⑤ 主函数通过接受用户输入，先后调用过滤函数、读取敏感词函数、创建文件函数，然后结束程序。

#### 考察知识

* 函数
* 文件操作
* 字符串操作

#### 实验代码

```python
#读取敏感词语文件
def read_sensitive_word():
    with open('sensitive_word.txt','r',encoding='utf-8') as file_to_read:
        lines = list()
        for line in file_to_read.readlines():
            if line is not None:
                #去除字符串前后的空格，然后将字符串写入列表中
                lines.append(line.strip('\n'))
    return lines
#创建文件，将过滤后的内容写入
def texcreat(name,text):
    file = open(name+'.txt','w',encoding='utf-8')
    file.write(text)
    file.close()
#过滤敏感词
def Replace(word):
    sensitive_word = read_sensitive_word()
    for line in sensitive_word:
        if line in word:
            #获取对应敏感词长度
            word_length = len(line)
            #将敏感词进行替换
            word = word.replace(line, '*' * word_length)
    return word
name = input("请输入文件名：")
sentence = input("请输入你想说的话：")
texcreat(name,Replace(sentence))
```

#### 实验结果

* 运行结果：
{% asset_img topic2.png # 结果 %}
* 敏感词文件：
{% asset_img sensitive.png # 敏感词 %}

### 题目三：电子抽奖器模拟

#### 题目内容

&emsp;&emsp;商店街上新开了一家超市，经营各种生活用品、食品、电器等。经营者是一对年轻夫妇， 为了纪念开业，他们举行了为期一个周的抽奖活动，并到处广告宣传，说这次抽奖平均 100 人就能有 1 人获得一等奖。首次宣传，促销期间门庭若市。 但最近商业街一些老店主跑来告状，说那家店在抽奖促销期间每天客流明显超过 100 人，然而一周过去了，抽中开业纪念大奖的只有 5 个人。请你模拟一下电子抽奖器，分析一下一周 5 个人中奖是否正常？

#### 解题思路

① 利用random函数来随机生成数用于模拟中奖情况；
② 循环多次来代替随机抽样，最后以数值型呈现出最终概率。

#### 实验设计

① 定义一个“抽奖函数”利用random库中的randint方法在0-9之间随机生成一个数，当数小于1时为真，大于1时为假，用于实现中奖概率为1%的条件；
② 在主函数中利用for循环循环多次实现模拟情况然后每次循环都调用“抽奖函数”，以其返回值作为判断依据然后统计最终的中奖次数。

#### 考察知识

* 函数
* random库

#### 实验代码

```python
import random
# 判断中奖函数
def lottery():
    flag = random.randint(0, 9)
    if flag < 1:
        return True
    else:
        return False

if __name__ == '__main__':
    # 中奖次数
    a = 0
    # 没有中奖次数
    b = 0
    for i in range(1000000):
        if (lottery()):
            a += 1
        else:
            b += 1
print('共计中奖：', a, '，未中奖：', b)
```

#### 实验结果

* 运行结果：
{% asset_img topic3.png # 结果 %}

### 题目四：小说《Walden》单词词频统计

#### 题目内容

&emsp;&emsp;Walden 中文译名《瓦尔登湖》，是美国作家梭罗独居瓦尔登湖畔的记录，描绘了他两年多时间里的所见、所闻和所思。该书崇尚简朴生活，热爱大自然的风光，内容丰厚，意义深远，语言生动。请用 Python 统计小说 Walden 中各单词出现的频次，并按频次由高到低排序。

#### 解题思路

① 读取文件；
② 为了避免因为大小写情况导致统计出错，先将所有大写字母转为小写字母；
③ 截取字符串，然后将单词和出现次数以键值对的形式存储在字典中然后输出。

#### 考察知识

* 字符串
* 文件操作
* 字典

#### 实验代码

```python
import re
#读取文件
file = open('Walden.txt','r')
text = file.read()
file.close()
#将所有英文字母变成小写
text = text.lower()
#使用正则表达式用空格替换标点符号
text = re.sub('[,.?:;"\']','',text)
#以空格为分隔符截取字符串
words = text.split(" ")
#创建统计单词的字典
word_sq = {}
#统计单词出现个数存入字典中
for i in words:
    if i not in word_sq.keys():
        word_sq[i] = 1
    else:
        word_sq[i] += 1
#按照字典的值进行排序
res = sorted(word_sq.items(),key=lambda x:x[1], reverse=True)
print("单词 出现次数")
for i in res:
    print(i[0],i[1])
```

#### 实验结果

* 运行结果：
{% asset_img topic4.png # 结果 %}