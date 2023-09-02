---
title: Python学习笔记（三）
date: 2021-03-22 08:37:48
categories:
- python
tags:
- study-notes
- python
---
程序控制结构是每一种程序设计语言都会涉及到的一块，这边文章就来讲讲python中的程序设计结构。

## 条件语句

### 用于条件组合的三个保留字

设x，y为两个条件：

| 操作符 | 描述 |
| :----: | :----: |
| x and y  | 逻辑与 |
| x or y | 逻辑或 |
| not x | 逻辑非 |

### 单分支结构

* 单分支结构的定义想必大家也比较清楚了。条件语句中单分支结构写法如下：
```python
if 判断条件:
    执行语句
```
**当条件符合时，执行相应的语句；不符合时则跳过。**

### 二分支结构

* 二分支增添了else这个保留字，和if一起使用，及**满足判断条件执行if中的内容，不满足就执行else中内容**：
```python
if 判断条件:
    执行语句
else：
    执行语句
```
### 多分支结构

* 多分支结构在二分支的结构上引入了elif这个保留字，实际上等价于C语言中的else if：
```python
if 判断条件:
    执行语句
elif 判断条件:
    执行语句
...
...
else:
    执行语句
```

## 循环语句
### for循环

* python中的for循环需要和保留字in进行搭配使用，即：
```python
for 元素 in 迭代对象:
    执行语句
```
* 这里我们要注意的是for语句是**从可迭代的对象中依次取出每一个元素，然后再进行操作。**
1. 和range()函数一起使用：
    - range()用于生成一个可迭代的对象
    - range(start, stop[, step])
```python
for i in range(3):
    print(i)
0
1
2
----------------------------
for i in range(1,3):
    print(i)
1
2
----------------------------
for i in range(1,5,2):
    print(i)
1
3
```
2. 列表，元组，集合，字符串
```python
#遍历列表
a = [1,2,3]
for i in a:
    print(i)
1
2
3
----------------------------
#遍历元组
b = ("hello","world","!")
for i in b:
    print(i)
hello
world
!
----------------------------
#遍历集合
c = {"hello","world","!"}
for i in c:
    print(i)
hello
world
!
----------------------------
#遍历字符串
d = "python"
for i in d:
    print(i)
p
y
t
h
o
n
```
3. 字典
```python
e = {'name':'sun','habit':'sunshine'}
#遍历键值对
for i,j in e.items():
    print(i,j)
name sun
habit sunshine
----------------------------
#遍历键
for i in e.keys():
    print(i)
name
habit
----------------------------
#遍历值
for i in e.values():
    print(i)
sun
sunshine
```
4. 文件
* fi是一个文件标识符，遍历其每行，产生循环
```python
for line in fi:
    <语句块>
```
### while循环

* 使用方法：
    - 当条件判断为True时，执行语句块；
    - 当条件判断为False时，循环终止。
```python
while <条件>：
    <语句块>
```

* 与else同用：
    - 当while语句条件为true时，执行语句块内容，为false时执行else语句中的内容。
```python
while <条件>:
    <语句块>
else:
    <语句块>
```

### 循环保留字：continue和break

#### continue
* 被用来跳过当前循环块中的剩余语句，然后继续进行下一轮循环

#### break
* 可以跳出 for 和 while 的循环体
* break仅跳出当前最内层循环
* 示例：
```python
for i in "PYTHON":
    if i == "T":
        continue
    print(i,end="")
    else:
        print("正常退出")
PYTHON正常退出
```
```python
for i in "PYTHON":
    if i == "T":
        break
    print(i,end="")
    else:
        print("正常退出")

```