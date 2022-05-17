---
title: 数据库实验五、六
date: 2021-04-28 11:10:44
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

* 了解数据库恢复技术的原理
* 了解oracle各类故障的数据恢复方法
* 了解oracle的物理备份
* 掌握oracle数据库逻辑备份方法
* 掌握oracle数据库恢复的方法
* 学会使用exp备份数据库、使用imp恢复数据库
* 了解flashback 的使用
* 学会使用PLSQL/developer工具完成导入导出
* 理解数据库的安全性保护
* 掌握ORACLE中有关用户创建的方法
* 理解数据库存取控制机制
* 熟练掌握PL-SQL的数据控制语言，能通过自主存取控制进行权限管理
* 熟悉用户资源文件的使用
* 熟悉ORACLE中角色管理
* 熟悉视图机制在自主存取控制上的应用

# 实验内容

1. 查看归档模式
2. 使用exp导出数据库
3. 使用imp导入数据库
4. 使用flashback 闪回表
5. 使用PLSQL/developer工具完成导出
6. 使用PLSQL/developer工具完成导入

# 实验步骤

## 实验五

1. 查看系统归档模式。（在SQLPLUS中）
SQL> archive log list。
对各参数值进行解释。
2. 热备份和冷备份，分别使用什么归档模式？
3. 如何对WINDOWS平台服务器中的ORCL数据库进行冷备份？说明方法。
4. 客户端可以使用RMAN进行热备份吗？
5. 逻辑备份
（1）导出自己表空间中的“预约”表
在运行中输入：exp 用户名/密码@orcl 
按照提示进行导出
（2）删除自己表空间中的“预约”表
（3）进行导入数据库操作
在运行中输入：IMP 用户名/密码@orcl
按照提示进行导入
（4）查询导入的“预约”表中的信息。
（5）导出数据库（以全库方式导出）。
* 必须是DBA才能执行完整数据库或表空间导出操作。
6. 使用Flashback
  （1）设置行可移动
SQL>ALTER TABLE 读者 ENABLE   ROW  MOVEMENT
（2）在读者表中添加多条记录（或者删除没有借书的读者记录）。
（3）闪回到改变前（TO_ TIMESTAMP函数完成对非时间戳类型数据的转换）
SQL>FLASHBACK TABLE   读者 TO TIMESTAMP   TO_ TIMESTAMP(….)
7. 使用PLSQL/developer 来完成SQL导出
（1）打开PLSQL/developer，选择菜单“工具“导出表
（2）点击你要导出的表，然后选择标签SQL 插入
（3）选中复选框创建表，浏览或者输入输出文件，然后点击导出
（4）在你输入的目录下找到你的导出文件（SQL 文件）
（5）依次导出你账户下所有用户自定义表。
（6）删除自己表空间中的“预约”表
（7）通过“工具“导入表，利用SQL插入导入数据库预约表。
（8）查询导入的预约表，检查导出是否正确。
8. 使用PLSQL/developer 来完成PLSQL/developer方式导出
（1）打开PLSQL/developer，选择菜单“工具“导出表
（2）点击你要导出的表，然后选择标签PLSQL/developer
（3）浏览或者输入输出文件，然后点击导出。
（4）在你输入的目录下找到你的导出文件。
（5）依次导出你账户下所有用户自定义表。
（6）删除自己表空间中的“预约”表
（7）通过“工具“导出表，PLSQL/developer方式导入数据库预约表。
（8）查询导入的预约表，检查导出是否正确。

## 实验六

1. 同学之间相互授权访问对方“读者”表并能进行查询。
2. 以SYS登录数据库 为你的账号增加系统角色DBA.
3. 重新以自己的账号登录，创建一个数据库用户：账号_USER1（注：账号即学生登录数据库账号：S2009XXXX）该用户拥有所有CONNECT, RESOURCE，DBA系统角色权限。然后以该用户登录，查看该用户的所有系统权限。
4. 建立角色：账号_OPER,该角色拥有调用存储过程借书、还书、预约的权限，以及查询读者，书目，图书，借阅以及预约表的权限。
（注：执行存储过程的授权语句
Grant execute on procedure_name  to user/role）
5. 创建一个数据库用户：账号_USER2（注：账号即：S2009XXXX）
为该用户授权角色：账户_OPER。以该用户登录，完成借书功能。
6. 建立视图VIEW_READER, 该视图包含书目（ISBN, 书名，作者，出版单位，图书分类名称）（注：所有属性来自关系书目和图书分类）
7. 创建一个数据库用户：账号_USER3（注：账号即：S2009XXXX）
该用户具有对视图VIEW_READER查询的权限。创建一个概要文件，如果 账号_USER3连续3次登录失败，则锁定该账户，10天后该账户自动解锁。以该用户登录进行权限测试。

# 实验结果

1. 查看系统归档模式。（在SQLPLUS中）SQL> archive log list。对各参数值进行解释。
使用SYS登录，由于是超级管理员所以要使用： “口令+ as sysdba”登录

{% asset_img pic1.png # tu %}

输入archive log list

{% asset_img pic2.png # tu %}

2. 热备份和冷备份，分别使用什么归档模式？
热备份针对归档模式的数据库，在数据库仍旧处于工作状态时进行备份。而冷备份是指在数据库关闭后，进行备份，适用于所有模式的数据库，热备份的优点在于当备份时，数据库仍旧可以被使用并且可以将数据库恢复到任意一个时间点。冷备份的优点在于他的备份和回复操作相当简单，并且由于冷备份的数据可以工作在非归档模式下，数据库性能会比归档模式稍好。
3. 如何对WINDOWS平台服务器中的ORCL数据库进行冷备份？说明方法。
1)正常关闭数据库
2)备份所有重要的文件到备份目录（数据文件、控制文件、重做日志文件等）
3)完成备份后启动数据库
4. 客户端可以使用RMAN进行热备份吗？ 可以
5. 逻辑备份
（1）导出自己表空间中的“预约”表在运行中输入：exp 用户名/密码@orcl 按照提示进行导出

{% asset_img pic3.png # tu %}

（2）删除自己表空间中的“预约”表

{% asset_img pic4.png # tu %}
{% asset_img pic5.png # tu %}

（3）进行导入数据库操作在运行中输入：IMP 用户名/密码@orcl，按照提示进行导入

{% asset_img pic6.png # tu %}
{% asset_img pic7.png # tu %}

（4）查询导入的“预约”表中的信息。

{% asset_img pic8.png # tu %}

（5）导出数据库（以全库方式导出）。
exp S5120188039/yyk1776572104@orcl FULL=Y

{% asset_img pic9.png # tu %}
{% asset_img pic10.png # tu %}
{% asset_img pic11.png # tu %}

6、使用Flashback
（1）设置行可移动
SQL>ALTER TABLE 读者 ENABLE ROW MOVEMENT

{% asset_img pic12.png # tu %}

（2）在读者表中添加多条记录（或者删除没有借书的读者记录）。
插入数据：

```SQL
INSERT INTO 读者 VALUES('20181452','胡晋源','四川绵阳西科大计算机学院','男',null,null,null);
INSERT INTO 读者 VALUES('20186666','吕硕','四川绵阳西科大计算机学院','男',null,null,null);
INSERT INTO 读者 VALUES('20188039','姚永坤','四川绵阳西科大计算机学院','男',null,null,null); 
```

（3）闪回到改变前（TO_ TIMESTAMP函数完成对非时间戳类型数据的转换）
SQL>FLASHBACK TABLE 读者 TO TIMESTAMP TO_ TIMESTAMP(….)
代码：

```SQL
FLASHBACK TABLE 读者 TO TIMESTAMP TO_TIMESTAMP('2020/11/15 16:30:00','YYYY/MM/DD HH24:MI:SS');
```

7、使用PLSQL/developer 来完成SQL导出
（1）打开PLSQL/developer，选择菜单“工具”导出表
{% asset_img pic13.png # tu %}

（2）点击你要导出的表，然后选择标签SQL插入
{% asset_img pic14.png # tu %}

（3）选中复选框创建表，浏览或者输入输出文件，然后点击导出
{% asset_img pic15.png # tu %}

（4）在你输入的目录下找到你的导出文件（SQL 文件）
（5）依次导出你账户下所有用户自定义表。

{% asset_img pic16.png # tu %}

（6）删除自己表空间中的“预约”表

```SQL
drop table 预约;
```

（7）通过“工具“导入表，利用SQL插入导入数据库预约表。

{% asset_img pic17.png # tu %}
{% asset_img pic18.png # tu %}

（8）查询导入的预约表，检查导出是否正确。

{% asset_img pic19.png # tu %}

8、使用PLSQL/developer 来完成PLSQL/developer方式导出
（1）打开PLSQL/developer，选择菜单“工具“导出表
（2）点击你要导出的表，然后选择标签PLSQL/developer

{% asset_img pic20.png # tu %}

（3）浏览或者输入输出文件，然后点击导出。

{% asset_img pic21.png # tu %}

（4）在你输入的目录下找到你的导出文件。
（5）依次导出你账户下所有用户自定义表。

{% asset_img pic22.png # tu %}

（6）删除自己表空间中的“预约”表

```SQL
drop table 预约;
```

（7）通过“工具“导出表，PLSQL/developer方式导入数据库预约表。

{% asset_img pic23.png # tu %}

（8）查询导入的预约表，检查导出是否正确

{% asset_img pic24.png # tu %}

## 实验六

1、同学之间相互授权访问对方“读者”表并能进行查询。
2、以SYS登录数据库 为你的账号增加系统角色DBA.

{% asset_img pic25.png # tu %}

使用 “GRANT DBA TO + 用户名”给自己的账号增加系统角色DBA：

{% asset_img pic26.png # tu %}

3、 重新以自己的账号登录，创建一个数据库用户：账号_USER1（注：账号即学生登录数据库账号：S2009XXXX）该用户拥有所有CONNECT, RESOURCE，DBA系统角色权限。然后以该用户登录，查看该用户的所有系统权限。
（1）登录

{% asset_img pic27.png # tu %}

（2）使用CREATE USER 用户+_USER1 IDENTIFIED BY ORCL;创建用户
```SQL
CREATE USER S5120188039_USER1 IDENTIFIED BY ORCL;
```

{% asset_img pic28.png # tu %}

（3）使用GRANT CONNECT TO S5120188039_USER1;
```SQL
GRANT RESOURCE TO S5120188039_USER1;
GRANT DBA TO S5120188039_USER1;
```
{% asset_img pic29.png # tu %}

4、 建立角色：账号_OPER,该角色拥有调用存储过程借书、还书、预约的权限，以及查询读者，书目，图书，借阅以及预约表的权限。
（注：执行存储过程的授权语句Grant execute on procedure_name  to user/role）
（1）创建角色
```SQL
CREATE ROLE S5120188039_OPER;
```
{% asset_img pic30.png # tu %}
（2）授权
```SQL
GRANT EXECUTE ON PROCESSED_借书 TO S5120188039_OPER;
GRANT EXECUTE ON PROCESSED_还书 TO S5120188039_OPER;
GRANT EXECUTE ON PROCEDURE_预约 TO S5120188039_OPER;
```
{% asset_img pic31.png # tu %}
```SQL
GRANT SELECT ON 读者 TO S5120188039_OPER;
GRANT SELECT ON 书目 TO S5120188039_OPER;
GRANT SELECT ON 图书 TO S5120188039_OPER;
GRANT SELECT ON 借阅 TO S5120188039_OPER;
GRANT SELECT ON 预约 TO S5120188039_OPER;
```
{% asset_img pic32.png # tu %}

5、创建一个数据库用户：账号_USER2（注：账号即：S2009XXXX）为该用户授权角色：账户_OPER。以该用户登录，完成借书功能。
（1）使用CREATE USER 用户+_USER1 IDENTIFIED BY ORCL;创建用户
```SQL
CREATE USER S5120188039_USER2 IDENTIFIED BY ORCL;
```
{% asset_img pic33.png # tu %}
（2）授权
```SQL
GRANT S5120188039_OPER TO S5120188039_USER2;
```
{% asset_img pic34.png # tu %}
（3）重新登录并且实现借书过程
{% asset_img pic35.png # tu %}
```SQL
CALL S5120188039.PROCESSED_借书(20051001,1005050);
```
{% asset_img pic36.png # tu %}
（4）查询结果
{% asset_img pic37.png # tu %}

7、 创建一个数据库用户：账号_USER3（注：账号即：S2009XXXX）
该用户具有对视图VIEW_READER查询的权限。创建一个概要文件，如果 账号_USER3连续3次登录失败，则锁定该账户，10天后该账户自动解锁。以该用户登录进行权限测试。
（1）创建数据库用户
```SQL
CREATE USER S5120188039_USER3 IDENTIFIED BY ORCL;
```
{% asset_img pic38.png # tu %}
（2）授权
```SQL
GRANT CONNECT TO S5120188039_USER3;
```
{% asset_img pic39.png # tu %}
```SQL
GRANT SELECT ON VIEW_READER TO S5120188039_USER3;
```
{% asset_img pic40.png # tu %}
（3）创建概要文件
```SQL
CREATE PROFILE LOCK_USER LIMIT
FAILED_LOGIN_ATTEMPTS 3
PASSWORD_LOCK_TIME 10;
```
{% asset_img pic41.png # tu %}
（4）分配概要文件
```SQL
ALTER USER S5120188039_USER3 PROFILE LOCK_USER;
```
{% asset_img pic42.png # tu %}
（5）测试输入错误，锁定用户
{% asset_img pic43.png # tu %}
{% asset_img pic44.png # tu %}
（6）账户锁定进行解锁
```SQL
ALTER USER S5120188039_USER3 ACCOUNT UNLOCK;
```
{% asset_img pic45.png # tu %}
（7）再次测试
{% asset_img pic46.png # tu %}