---
title: Python 实验十三 数据分析与可视化（2）
date: 2021-04-05 18:00:21
tags:
- python-experiment
- experiment
categories:
- experiment
---

## 实验目的
1、 掌握第三方库 TuShare 的数据获取方法
2、 能够利用 pandas 库进行简单的数据分析
3、 能够利用 matplotlib 库进行数据可视化
4、 综合应用上述第三方库解决问题的能力
## 实验内容
### 一、读取 stock_hist_data.csv 中招商银行（股票代码 600036）2018 年下半年的股票数据并完成如下数据处理和分析任务：
##### (1) 数据只保留 date、open、high、close、low 和 volume 这几个属性，并按时间先后顺序对数据进行排序；使用 matplotlib 绘制出收盘价（close）的走势折线图。
##### (2) 输出这半年内成交量（volume）最低和最高那两天的日期和分别的成交量；
##### (3) 列出成交量（volume）在 1000000 以上的记录；
##### (4) 计算这半年中收盘价（close）高于开盘价（open）的天数；
##### (5) 计算每月收盘价的平均值，并使用 matplotlib 绘制出柱状图。
#### 代码

```python
import pandas as pd
import matplotlib.pyplot as plt
plt.rcParams['font.family'] = 'SimHei'
#(1) 数据只保留 date、open、high、close、low 和 volume 这几个属性，并按时间先后顺序对数据进行排序；使用 matplotlib 绘制出收盘价（close）的走势折线图。
data_frame = pd.DataFrame(pd.read_csv('stock_hist_data.csv').iloc[:,:6])

data_frame = data_frame.sort_values('date',ascending=True)

data_frame.close.plot()

plt.title("收盘价(close)的走势图")
plt.savefig('收盘价(close)的走势图.jpg')
plt.show()

#（2）输出这半年内成交量（volume）最低和最高那两天的日期和分别的成交量；
data_volume_max_index = data_frame.volume.idxmax()
data_volume_max = data_frame.loc[data_volume_max_index][['date','volume']].values
# print(data_volume_max_index)
data_volume_min_index = data_frame.volume.idxmin()
data_volume_min = data_frame.loc[data_volume_min_index][['date','volume']].values

print("半年成交量中{}这一天最高，为{}。\n".format(data_volume_max[0],data_volume_max[1]))
print("半年成交量中{}这一天最低，为{}。\n".format(data_volume_min[0],data_volume_min[1]))

#(3) 列出成交量（volume）在 1000000 以上的记录；
data_volume_more_than_1000000 = data_frame[data_frame.volume > 1000000]
print("成交量在1000000以上的记录为：\n",data_volume_more_than_1000000)

#(4) 计算这半年中收盘价（close）高于开盘价（open）的天数；
data_frame_count = data_frame[data_frame.close > data_frame.open]['date'].count()
print("收盘价高于开盘价的天数共有：{}天\n".format(data_frame_count))

#(5) 计算每月收盘价的平均值，并使用 matplotlib 绘制出柱状图。
data_frame_5 = data_frame.loc[:,['date','close']]
data_frame_5['date'] = data_frame_5['date'].apply(lambda x:x[:7])
data_frame_5_close_average = data_frame_5.groupby('date').mean()
# print(data_frame_5_close_average)
data_frame_5_close_average.plot.bar()
plt.title("每月收盘价(close)的平均值")
plt.savefig('average.jpg')
plt.show()
```
#### 结果
（1）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210112224752413.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzk0Nzk0Mw==,size_16,color_FFFFFF,t_70)
（5）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210112224835490.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzk0Nzk0Mw==,size_16,color_FFFFFF,t_70)
### 二、读取“2018 世界杯球队数据.csv”，实现如下功能：
##### (1) 输出净胜球大于 0 的球队；
##### (2) 输出被罚红牌的球队；
##### (3) 输出进球成功率超过 10%的球队以及进球数和射门数；
##### (4) 输出进球数超过进球平均数且被罚黄牌少于 5 张的球队及其进球数和黄牌数；
##### (5) 按照进球数降序输出所有球队及进球信息；
##### (6) 按照所属区进行分组，按升序统计输出每个区的进球数；
##### (7) 按照所属区进行分组，绘制每个区进球数的条形图；
##### (8) 自选角度，根据各个球队的相关数据，绘制一个其他类型的图形，并增加一些必要的元素（如标签、标题等）。
#### 代码

```python
import pandas as pd
import matplotlib.pyplot as plt
plt.rcParams['font.family'] = 'SimHei'
#(1) 输出净胜球大于 0 的球队；
data_frame = pd.DataFrame(pd.read_csv('2018世界杯球队数据.csv',encoding='gbk'))
data_frame_1 = data_frame.loc[:,['球队','进球','失球']]
data_frame_1_win_goal = data_frame_1['进球'] - data_frame_1['失球']
data_frame_1['净胜球'] = data_frame_1_win_goal
data_frame_1 = data_frame_1[data_frame_1.净胜球 >0]
data_frame_1_final = data_frame_1.loc[:,['球队','净胜球']]
data_frame_1_name = data_frame_1_final['球队'].values
print('净胜球大于0的球队有：',end='')
for i in data_frame_1_name:
    print(i,end=' ')

#(2) 输出被罚红牌的球队；
data_frame_2 = data_frame.loc[:,['球队','红牌']]
data_frame_2 = data_frame_2[data_frame_2.红牌 > 0].values
print('被罚红牌的队伍有:')
for i in data_frame_2:
    print(i[0],end=' ')

#（3）输出进球成功率超过 10%的球队以及进球数和射门数；
data_frame_3 = data_frame.loc[:,['球队','进球','射门']]
data_frame_3_goalrate = data_frame_3['进球'] / data_frame_3['射门']*100
data_frame_3['进球率'] = data_frame_3_goalrate
data_frame_3 = data_frame_3[data_frame_3.进球率 > 10]
data_frame_3.drop(labels='进球率',axis=1,inplace=True)
print(data_frame_3)

#(4) 输出进球数超过进球平均数且被罚黄牌少于 5 张的球队及其进球数和黄牌数；
data_frame_4 = data_frame.loc[:,['球队','进球','黄牌']]
data_frame_4_TotalGoalAverage = data_frame_4['进球'].mean()
# print(data_frame_4_TotalGoalAverage)
data_frame_4 = data_frame_4[(data_frame_4.进球 > data_frame_4_TotalGoalAverage) & (data_frame_4.黄牌 < 5)]
print(data_frame_4)

#(5) 按照进球数降序输出所有球队及进球信息；
data_frame_5 = data_frame.loc[:,['球队','进球']]
data_frame_5.sort_values(by='进球',ascending=False,inplace=True)
print(data_frame_5)

#(6) 按照所属区进行分组，按升序统计输出每个区的进球数；
data_frame_6 = data_frame.loc[:,['所属洲','进球']]
data_frame_6_final = data_frame_6.groupby('所属洲').sum()
data_frame_6_final.sort_values(by='进球',inplace=True)
print(data_frame_6_final)

#(7) 按照所属区进行分组，绘制每个区进球数的条形图；
data_frame_7 = data_frame.loc[:,['所属洲','进球']]
data_frame_7_final = data_frame_7.groupby('所属洲').sum()
data_frame_7_final.plot.bar()
plt.title('各大洲进球数')
plt.show()

#(8) 自选角度，根据各个球队的相关数据，绘制一个其他类型的图形，并增加一些必要的元素（如标签、标题等）。
data_frame_8 = data_frame.loc[:,['所属洲','球队','进球','抢断','射门']]
data_frame_8 = data_frame_8[data_frame_8.所属洲 == '欧洲']
data_frame_8_bar = data_frame_8.loc[:,['球队','进球']]
data_frame_8_pie = data_frame_8.loc[:,['球队','抢断']]
data_frame_8_plot = data_frame_8.loc[:,['球队','射门']]
# print(data_frame_8_bar)
plt.subplot(221)
plt.title('欧洲球队抢断数')
plt.scatter(data_frame_8_pie['抢断'],data_frame_8_pie['球队'])
plt.subplot(222)
plt.title('欧洲球队射门次数占比')
plt.pie(data_frame_8_plot['射门'],labels=data_frame_8_plot['球队'])
plt.subplot(212)
plt.title('欧洲球队进球数')
plt.bar(data_frame_8_bar['球队'],data_frame_8_bar['进球'])
plt.show()
```
#### 结果
（6）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210112230241943.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzk0Nzk0Mw==,size_16,color_FFFFFF,t_70)
（7）
￼![在这里插入图片描述](https://img-blog.csdnimg.cn/2021011223033649.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzk0Nzk0Mw==,size_16,color_FFFFFF,t_70)

