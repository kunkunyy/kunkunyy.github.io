---
title: 数据库实验（一）建立数据库
date: 2021-04-07 12:19:09
tags:
- Database-experiment
- experiment
categories:
- [experiment]
- [数据库实验]
---

# 实验类型

设计性实验

# 实验目的

* 熟悉oracle环境。
* 熟练掌握和使用PL-SQL建立数据库基本表。
* 使用PL/SQL developer操作数据库。
* 熟练掌握SQL 建立关系，及增删改数据。

# 实验内容

* 了解SQL PLUS的使用
* 使用PL/SQL developer的图形界面，建立图书管理数据库orcl中的读者关系，要求创建主键约束，用户定义的完整性约束（电话号码、身份证号）。
* 在建立的读者关系中输入有效数据。
* 删除读者关系。
* 在PL/SQL developer用SQL代码建立orcl数据库中各关系。
* 用SQL 代码完成数据增、删、改。

# 实验步骤

## 登录数据库

1. 以SYSTEM登录数据库

* 输入口令为你创建数据库时设置的密码

{% asset_img pic1.png # tu %}

2. 注册用户、重新以新用户登录数据库

{% asset_img pic2.png # tu %}

## 图形化界面建表

1. 图书分类（图书分类号，类名）

{% asset_img pic3.png # tu %}
{% asset_img pic4.png # tu %}
{% asset_img pic5.png # tu %}

2. 读者 （借书证号，姓名，单位，性别，地址，联系电话，身份证编号）

{% asset_img pic6.png # tu %}
{% asset_img pic7.png # tu %}
{% asset_img pic8.png # tu %}

3. 书目（ISBN, 书名，作者，出版单位，单价，图书分类号）

{% asset_img pic9.png # tu %}
{% asset_img pic10.png # tu %}
{% asset_img pic11.png # tu %}

4. 图书（图书编号，ISBN，是否借出，备注）

{% asset_img pic12.png # tu %}
{% asset_img pic13.png # tu %}
{% asset_img pic14.png # tu %}

5. 借阅 （借阅流水号，借书证号，图书编号，借书日期，归还日期，罚款分类号，备注）

{% asset_img pic15.png # tu %}
{% asset_img pic16.png # tu %}
{% asset_img pic17.png # tu %}

6. 罚款分类（罚款分类号，罚款名称，罚金）

{% asset_img pic18.png # tu %}
{% asset_img pic19.png # tu %}
{% asset_img pic20.png # tu %}

7. 预约（预约流水号，借书证号，ISBN，日期）

{% asset_img pic21.png # tu %}
{% asset_img pic22.png # tu %}
{% asset_img pic23.png # tu %}

## 建立数据库表：打开tables文件夹。建立以下各关系：

1. 图书分类（图书分类号，类名）

| 图书分类号 | 类名 |
| :----: | :----: |
| 100 | 文学 |
| 200 | 科技 |
| 300 | 哲学 |

```SQL
create table 图书分类
(
  图书分类号 NUMBER primary key,
  类名    VARCHAR2(4)
)

insert into 图书分类 (图书分类号,类名) values (100,'文学');
insert into 图书分类 values (200,'科技');
insert into 图书分类 (图书分类号,类名) values (300,'哲学');
```

2. 书目（ISBN, 书名，作者，出版单位，单价，图书分类号）

| ISBN | 书名 | 作者 | 出版单位 | 单价 | 图书分类号 |
| :----: | :----: | :----: | :----: | :----: | :----: |
| 7040195836 | 数据库系统概论 | 王珊 | 高等教育出版社 | 39.00 | 200 |
| 9787508040110	| 红楼梦 | 曹雪芹 | 人民出版社 | 20.00 | 100 |
| 9787506336239 | 红楼梦 | 曹雪芹 | 作家出版社 | 34.30 | 100 |
| 9787010073750	| 心学之路 | 张立文	| 人民出版社 | 33.80 | 300 |

```SQL
create table 书目
(
  isbn  NUMBER primary key
  书名    VARCHAR2(14),
  作者    VARCHAR2(6),
  出版单位  VARCHAR2(14),
  单价    NUMBER,
  图书分类号 NUMBER,
  出版年份  NUMBER,
  foreign key(图书分类号) references 图书分类(图书分类号)
)

insert into 书目 values (7040195836,'数据库系统概论','王珊','高等教育出社',39.00,'200');
insert into 书目 values (9787508040110,'红楼梦','曹雪芹','人民出版社',20.00,'100');
insert into 书目 values (9787506336239,'红楼梦','曹雪芹','作家出版社',34.30,'100');
insert into 书目 values (9787010073750,'心学之路','王珊','人民出版社',33.80,'300');
```

3. 图书（图书编号，ISBN，是否借出，备注）

| 图书编号 | ISBN | 是否借出 | 备注 |
| :----: | :----:| :----: | :----:|
| 2001231 | 7040195836 | 否 |   |
| 2001232 | 7040195836 | 是	|   |
| 1005050 | 9787506336239 | 否 |   |	
| 1005063 | 9787508040110 | 是 |   |
| 3007071 | 9787010073750 | 是 |   |

```SQL
create table 图书
(
  图书编号 NUMBER primary key,,
  isbn NUMBER,
  是否借出 VARCHAR2(2),
  备注   VARCHAR2(24),
  foreign key(ISBN) references 书目(ISBN)
)

insert into 图书 values (2001231,'7040195836','否',null);
insert into 图书 values (2001232,'7040195836','是',null);
insert into 图书 values (1005050,'9787506336239','否',null);
insert into 图书 values (1005063,'9787508040110','是',null);
insert into 图书 values (3007071,'9787010073750','是',null);
```

4. 读者 （借书证号，姓名，单位，性别，地址，联系电话，身份证编号）

| 借书证号 | 姓名 | 单位 | 性别 | 地址 | 联系电话 | 身份证编号 |
| :----: | :----: | :----: | :----: | :----: | :----: | :----: |
| 20051001 | 王菲 | 四川绵阳西科大计算机学院 | 女 | … | … | .. |
| 20062001 | 张江 | 四川绵阳中心医院 | 男 | … | … | .. |
| 20061234 | 郭敬明 | 四川江油305 | 男 | .. | .. | .. |
| 20071235 | 李晓明 | 四川成都工商银行 | 男 | .. | .. | .. |
| 20081237 | 赵鑫 | 四川广元广元中学 | 女 | .. | .. | .. |

```SQL
create table 读者
(
  借书证号  VARCHAR2(10) primary key,
  姓名    VARCHAR2(10),
  单位    VARCHAR2(25),
  性别    VARCHAR2(2),
  地址    VARCHAR2(16),
  联系电话  VARCHAR2(16),
  身份证编号 VARCHAR2(16)
)

insert into 读者 values ('20051001','王菲','四川绵阳西科大计算机学院','女',null,null,null);
insert into 读者 values ('20062001','张江','四川绵阳中心医院','男','绵阳',null,null,null);
insert into 读者 values ('20061234','郭敬明','四川江油305','男','四川',null,null,null);
insert into 读者 values ('20071235','李晓明','四川成都工商银行','男','成都',null,null,null);
insert into 读者 values ('20081237','赵鑫','四川广元广元中学','女','广元',null,null,null);
```

5. 借阅 （借阅流水号，借书证号，图书编号，借书日期，归还日期，罚款分类号，备注）

| 借书证号 | 图书编号 | 借书日期 | 归还日期 | 罚款分类号 | 备注 |
| :----: | :----: | :----: | :----: | :----: | :----: |
| 20081237 | 3007071 | 2010/09/19 | 2010/09/20 |   |   |
| 20071235 | 1005063 | 2010/10/20 | 2011/02/20 | 1 |   |
| 20071235 | 2001232 | 2011/09/01 |   |   |   |
| 20061234 | 1005063 | 2011/9/20 |   |   |   |
| 20051001 | 3007071 | 2011/9/10 |   |   |   |
| 20071235 | 1005050 | 2011/10/20 | 2012/02/20 | 1 |   |

```SQL
create table 借阅
(
  借阅流水号 NUMBER primary key,
  借书证号  VARCHAR2(16),
  图书编号  NUMBER(10),
  借书日期  DATE,
  归还日期  DATE,
  罚款分类号 NUMBER,
  备注    VARCHAR2(20),
  foreign key(借书证号) references 读者(借书证号)
  foreign key(图书编号) references 图书(图书编号)
  foreign key(罚款分类号) references 罚款分类(罚款分类号)
)

insert into 借阅 values (1,'20081237',3007071,to_date('2010-09-19','yyyy/mm/dd'),to_date('2010-09-20','yyyy/mm/dd'),null,null);
insert into 借阅 values (2,'20071235',1005063,to_date('2010-10-20','yyyy/mm/dd'),to_date('2011-02-20','yyyy/mm/dd'),1,null);
insert into 借阅 values (3,'20071235',2001232,to_date('2011-09-01','yyyy/mm/dd'),null,null,null);
insert into 借阅 values (4,'20061234',1005063,to_date('2011-09-20','yyyy/mm/dd'),null,null,null);
insert into 借阅 values (5,'20051001',3007071,to_date('2011-09-10','yyyy/mm/dd'),null,null,null);
insert into 借阅 values (6,'20071235',1005050,to_date('2011-10-20','yyyy/mm/dd'),to_date('2012/02/20','yyyy/mm/dd'),1,null);
```

6. 罚款分类（罚款分类号，罚款名称，罚金）

| 罚款分类号 | 罚款名称 | 罚金 |
| :----: | :----: | :----: |
| 1 | 延期 | 10 |
| 2	| 损坏 | 20 |
| 3	| 丢失 | 50 |

```SQL
create table 罚款分类
(
  罚款分类号 NUMBER primary key,
  罚款名称  VARCHAR2(10),
  罚金    NUMBER(3)
)

insert into 罚款分类 values (1,'延期',10);
insert into 罚款分类 values (2,'损坏',20);
insert into 罚款分类 values (3,'丢失',50);
```

7. 预约 （预约流水号，借书证号，ISBN，预约时间）

| 预约流水号 | 借书证号 | ISBN | 预约时间 |
| :----: | :----: | :----: | :----: |
| 1 | 20081237 | 9787508040110 | 2011/09/11 |

```SQL
create table 预约
(
  预约流水号  NUMBER primary key,
  借书证号   VARCHAR2(16),
  isbn   NUMBER,
  s_date DATE
)

insert into 预约 values (1,'20081237','9787508040110','2011/09/11','yyyy/mm/dd');
```

## 使用SQL语句练习表的创建、删除、修改操作

```SQL
select * from 图书;

alter table 图书 add 图书分类号 number;

alter table 图书 drop column 图书分类号;

update 图书 set 是否借出='否' where 图书分类号='2002141';
```

## 试根据下面的完整性约束要求，用SQL对上面已经建立好的数据库表进行完整性约束定义。

读者关系中属性  联系电话  取值为11位数字
身份证编号  取值为18位，并且满足身份证编号规则

```SQL
alter table 读者 add constraint PhoneNumber check(regexp_like(联系电话,'1[3|4|5|7|8][0-9]{9}'));

alter table 读者 add constraint IDENTITYNUM check(regexp_like(身份证号,'[1-9\d{5}(19|20)\d{2}((0[1-9])|(1[0-2]))(([0-2][1-9])|([1-3]0)|31)\d{3}[0-9Xx]'))
```