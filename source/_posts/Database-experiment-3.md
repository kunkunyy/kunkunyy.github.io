---
title: 数据库实验三、四
date: 2021-04-28 11:10:20
tags:
- Database-experiment
- experiment
categories:
- [experiment]
- [数据库实验]
---

# 实验类型

验证型实验

# 实验目的
 
* 了解存储过程的概念、优点
* 熟练掌握创建存储过程的创建方法
* 熟练掌握存储过程的调用方法

# 实验内容

1. 建立存储过程
2. 调用存储过程

# 实验步骤

1. 建立存储过程完成图书管理系统中的借书功能，功能要求：
* 借书时要求输入借阅流水号，借书证号，图书编号。（即该存储过程有3个输入参数）
* 借书时，借书日期为系统时间。
* 图书的是否借出改为‘是’
2. 建立存储过程完成图书管理系统中的预约功能。
* 预约时要求输入预约流水号，借书证号，ISBN。（即该存储过程有3个输入参数）
* 存储过程先检查输入的ISBN版本的图书是否都已借出，如果是则进行预约，否则提示“该书目有可借图书，请查找”。
* 预约时间为系统时间。
3. 建立存储过程完成图书管理系统中的还书功能。
* 还书时要求输入借书证号，图书编号，罚款分类号（即该存储过程有3个输入参数）。
* 还书日期为系统时间。
* 图书的是否借出改为‘否’。

1. 通过序列和触发器实现借阅表中借阅流水号字段的自动递增。
2. 通过序列和触发器实现预约表中预约流水号字段的自动递增
3. 修改实验三借书功能的存储过程。该存储过程要求：
（1）借书时输入借书证号，图书编号。（即该函数有2个输入参数）
（2）借书时，借书日期为系统时间。
    * 该存储过程主体部分只有insert into语句。
4. 建立与借书存储过程相对应的触发器，当借阅表中加入借阅信息时，该触发器触发，自动修改所借图书的是否借出改为‘是’。
5. 修改实验三还书功能的存储过程。该存储过程要求：
（1）还书时输入借书证号，图书编号。（即该函数有2个输入参数）
（2）还书时，还书日期为系统时间。
* 该存储过程主体部分只有一条UPDATE语句。
6. 建立与还书存储过程相对应的触发器，当借阅表中填入还书日期时，该触发器触发，自动修改所还图书的是否借出为‘否’。

# 实验结果

## 实验三

1. 建立存储过程完成图书管理系统中的借书功能，功能要求：
* 借书时要求输入借阅流水号，借书证号，图书编号。（即该存储过程有3个输入参数）
* 借书时，借书日期为系统时间。
* 图书的是否借出改为‘是’

```SQL
create or replace procedure Procedure_借书
(
vis_借阅流水号 in 借阅.借阅流水号%type,
vis_借书证号 in 借阅.借书证号%type,
vis_图书编号 in 借阅.图书编号%type
)
as
begin
  insert into 借阅
  values(vis_借阅流水号,vis_借书证号,vis_图书编号,sysdate,null,null,null);
  update 图书
  set 图书.是否借出 = '是'
  where 图书.图书编号 = vis_图书编号;
end;
```

2. 建立存储过程完成图书管理系统中的预约功能。
* 预约时要求输入预约流水号，借书证号，ISBN。（即该存储过程有3个输入参数）
* 存储过程先检查输入的ISBN版本的图书是否都已借出，如果是则进行预约，否则提示“该书目有可借图书，请查找”。
* 预约时间为系统时间。

```SQL
create or replace procedure Procedure_预约
(
vis_预约流水号 in 预约.预约流水号%type,
vis_借书证号 in 预约.借书证号%type,
vis_ISBN in 预约.ISBN%type
)
as
vis_数量 number;
begin
  select count(*) into vis_数量 from 图书
  where 图书.ISBN=vis_ISBN
  and 图书.是否借出='否';
  if vis_数量= 0 then
    insert into 预约 values(vis_预约流水号,vis_借书证号,vis_ISBN,sysdate);
    commit;
  else
    dbms_output.put_line('该书目有可借图书，请查找！');
  end if;
end;
```

3. 建立存储过程完成图书管理系统中的还书功能。
* 还书时要求输入借书证号，图书编号，罚款分类号（即该存储过程有3个输入参数）。
* 还书日期为系统时间。
* 图书的是否借出改为‘否’。

```SQL
create or replace procedure Procedure_还书
(
vis_借书证号 in 借阅.借书证号%type,
vis_图书编号 in 借阅.图书编号%type,
vis_罚款分类号 in 借阅.罚款分类号%type
)
as
begin
  update 借阅
  set 借阅.归还日期=sysdate,借阅.罚款分类号=vis_罚款分类号
  where 借阅.借书证号=vis_借书证号
  and 借阅.图书编号=vis_图书编号;
  update 图书 set 图书.是否借出='否'
  where 图书.图书编号=vis_图书编号;
end;
```

## 实验四

1. 通过序列和触发器实现借阅表中借阅流水号字段的自动递增。

```SQL
create sequence SEQ_序列
minvalue 1
maxvalue 1.0E28
start with 1
increment by 1
cache 20;

create or replace trigger TR_借阅流水号自增
       before insert on 借阅
       for each row
begin
  select SEQ_序列.NEXTVAL
  into :new.借阅流水号
  from dual;
end;
```

2. 通过序列和触发器实现预约表中预约流水号字段的自动递增

```SQL
create or replace trigger TR_预约流水号自增
       before insert on 预约
       for each row
begin
  select SEQ_序列.NEXTVAL
  into :new.预约流水号
  from dual;
end;
```

3、 修改实验三借书功能的存储过程，该存储过程要求：
（1）借书时输入借书证号，图书编号。（即该函数有2个输入参数）
（2）借书时，借书日期为系统时间。
* 该存储过程主体部分只有insert into语句。

```SQL
create or replace procedure Processed_借书   
(  
vis_借书证号 in 借阅.借书证号%type,  
vis_图书编号 in 借阅.图书编号%type  
)  
as vis_是否借出 图书.是否借出%type;
begin
  select 是否借出 into vis_是否借出 from 图书 where vis_图书编号 = 图书.图书编号;
  if vis_是否借出='是' then
   dbms_output.put_line('该书已经被借走了！');
  else  
   select 图书.是否借出  
   into vis_是否借出  
   from 图书  
   where 图书.图书编号=vis_图书编号; 
   insert into 借阅(借书证号,图书编号,借书日期)  
   values(vis_借书证号,vis_图书编号,sysdate);
   commit;  
  end if;   
end;
```

4. 建立与借书存储过程相对应的触发器，当借阅表中加入借阅信息时，该触发器触发，自动修改所借图书的是否借出改为‘是’。

```SQL
create or replace trigger TR_借书
       after insert on 借阅
       for each row
begin
  update 图书 set 是否借出 = '是' where 图书编号 = :new.图书编号;
end;
```

5、 修改实验三还书功能的存储过程，该存储过程要求：
（1）还书时输入借书证号，图书编号，罚款分类号。（即该函数有3个输入参数）
（2）还书时，还书日期为系统时间。
* 该存储过程主体部分只有一条UPDATE语句。

```SQL
create or replace procedure Processed_还书 
(
vis_借书证号 in 借阅.借书证号%type,
vis_图书编号 in 借阅.图书编号%type
)
as vis_是否借出 图书.是否借出%type;
begin
  update 借阅 set 借阅.归还日期 = sysdate()
  where 借阅.借书证号 = vis_借书证号
  and 借阅.图书编号 = vis_图书编号
  and 借阅.归还日期 is null;
end;
```

6. 建立与还书存储过程相对应的触发器，当借阅表中填入还书日期时，该触发器触发，自动修改所还图书的是否借出为‘否’。

```SQL
create or replace trigger TR_还书
after update on 借阅
for each row
  begin
    update 图书 set 是否借出 = '否'
    where 图书编号 = :new.图书编号;
end;
```