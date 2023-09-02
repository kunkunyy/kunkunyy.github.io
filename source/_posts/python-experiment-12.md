---
title: Python 实验十二 数据分析与可视化（1）
date: 2021-04-05 17:57:29
tags:
- python-experiment
- experiment
categories:
- experiment
---

# 实验目的
1、 熟悉 numpy 库常用方法的使用
2、 熟悉 pandas 库的基本使用
3、 能够利用 matplotlib 库进行简单的图形绘制
# 实验内容
### 题目
根据某商品近 5 年的销售流水，做数据分析和可视化。模拟产生数据代码如下：
￼![在这里插入图片描述](https://img-blog.csdnimg.cn/20210112222109186.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzk0Nzk0Mw==,size_16,color_FFFFFF,t_70)
### （1）编写程序生成某商品(2014-01-01 到 2018-12-31)的销售流水，模拟数据文件名为 data.csv，数据格式如下：（说明：日期是连续的，销量是随机数，单价范围为[101，105]的随机值）
￼![在这里插入图片描述](https://img-blog.csdnimg.cn/20210112222332749.png)
#### 代码
```python
import random
import datetime
import csv

fn = 'data.csv'
with open(fn,'w') as fp:
    wr = csv.writer(fp)
    wr.writerow(['日期','销量','单价'])
    startDate = datetime.date(2014,1,1)
    for i in range(1825):
        amount = 300+i*5+random.randrange(100)
        price = 100+random.randint(1,5)
        wr.writerow([str(startDate),amount,price])
        startDate = startDate+datetime.timedelta(days=1)
```
### （2）使用 pandas 读取文件 data.csv 中的数据，创建 DataFrame 对象，并删除其中所有缺失值；
#### 代码
```python
import pandas as pd

df = pd.DataFrame(pd.read_csv('data.csv',encoding='gbk'))

df.dropna()
```
### （3）使用 matplotlib 生成折线图，反映每月的销量情况，并把图形保存为本地文件 one.jpg；
#### 代码
```python
import matplotlib.pyplot as plt
import pandas as pd
plt.rcParams['font.family'] = 'SimHei'

df = pd.DataFrame(pd.read_csv('data.csv',encoding='gbk'))
df.dropna()
df['日期'] = df['日期'].apply(lambda x: x[:7])
# print(df['日期'])
data_quantity = df.iloc[:,0:2]
# print(data_quantity)
group_month_quantity = data_quantity.groupby('日期').sum()
# print(group_month_quantity)
group_month_quantity.plot()
plt.savefig('one.jpg')
plt.show()
```
#### 结果
￼![在这里插入图片描述](https://img-blog.csdnimg.cn/20210112223126235.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzk0Nzk0Mw==,size_16,color_FFFFFF,t_70)
### （4）按年进行统计，使用 matplotlib 绘制柱状图显示每年的营业额，并把图形保存为本地文件 two.jpg；
#### 代码
```python
import matplotlib.pyplot as plt
import pandas as pd
plt.rcParams['font.family'] = 'SimHei'

df = pd.DataFrame(pd.read_csv('data.csv',encoding='gbk'))
df.dropna()
df['日期'] = df['日期'].apply(lambda x: x[:4])
# print(df['日期'])
data_quantity = df.iloc[:,0:2]
group_month_quantity = data_quantity.groupby('日期').sum()
# print(group_month_quantity)
group_month_quantity.plot.bar()
plt.savefig('two.jpg')
plt.show()
```
#### 结果
￼![在这里插入图片描述](https://img-blog.csdnimg.cn/20210112223253561.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzk0Nzk0Mw==,size_16,color_FFFFFF,t_70)
### （5）按年进行统计，使用 matplotlib 绘制柱状图显示每年销售量最大的月份及销售额，并把图形保存为本地文件 three.jpg；
#### 代码
```python
import matplotlib.pyplot as plt
import pandas as pd
plt.rcParams['font.family'] = 'SimHei'

df = pd.DataFrame(pd.read_csv('data.csv',encoding='gbk'))
df.dropna()
df['日期'] = df['日期'].apply(lambda x: x[:4])
# print(df['日期'])
data_quantity = df.iloc[:,0:2]
# print(data_quantity)
group_month_quantity = data_quantity.groupby('日期').max()
# print(group_month_quantity)
group_month_quantity.plot.bar()
plt.savefig('three.jpg')
plt.show()
```
#### 结果
￼![在这里插入图片描述](https://img-blog.csdnimg.cn/20210112223346284.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzk0Nzk0Mw==,size_16,color_FFFFFF,t_70)

### （6）按年度统计该商品的营业额数据，使用 matplotlib 生成饼状图显示每年都的营业额分布情况，并把图形保存为本地文件 four.jpg。
#### 代码
```python
import matplotlib.pyplot as plt
import pandas as pd
plt.rcParams['font.family'] = 'SimHei'

df = pd.DataFrame(pd.read_csv('data.csv',encoding='gbk'))
df.dropna()
df['日期'] = df['日期'].apply(lambda x: x[:4])
# print(df['日期'])
data_quantity = df.iloc[:,0:2]
# print(data_quantity)
group_month_quantity = data_quantity.groupby('日期').sum()
# print(group_month_quantity)
group_month_quantity.plot.pie(subplots=True)
plt.savefig('four.jpg')
plt.show()
```
#### 结果
￼![在这里插入图片描述](https://img-blog.csdnimg.cn/20210112223440838.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzk0Nzk0Mw==,size_16,color_FFFFFF,t_70)
