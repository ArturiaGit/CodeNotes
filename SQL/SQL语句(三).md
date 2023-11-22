---
title: SQL语句(三)
date: 2023-11-21 18:47:19
description: 该篇文章主要介绍了如何使用SQL语句对基本表进行定义、删除与修改
categories:
  - - 数据库系统概论
    - SQL教程
tags:
  - 基本表
  - SQL语句
top_image: https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202311211942767.png
cover: https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202311211942767.png
keywords:
  - 定义
  - 删除
  - 修改
---
# 基本表的定义、删除与修改
## <font color = "886600">基本表的定义</font>
- 语句格式如下：
	- CREATE TABLE <表名></br>(<列名><数据类型>\[列级完整性约束条件>])</br>\[,<列名><数据类型>\[列级完整性约束条件]]</br>……</br>\[,<表级完整性约束条件>]);
- 说明：
	- CREATE TABLE <表名>：表示创建一张表，表名随意（注意，<>不需要写上，这里我只是做个格式区分而已）
	- ()：括号里用于定义列名，列的数据类型以及是否具有完整性约束
	- \[]：\[]里的内容可有可无，不是必需要的
	- 如果完整性约束条件涉及到该表的多个属性列，则必须定义在表级上，否则既可以定义在列级上也可以定义在表级上
- 例：建立“学生”表Student，学号是主码，姓名取值唯一
	- 语句如下：
		- CREATE TABLE Student</br>(Sno CHAR(9) PRIMARY KEY,</br>Sname CHAR(20) UNIQUE,</br>Ssex CHAR(2),</br>Sage SMALLINT,</br>Sdept CHAR(20));
	- 说明：
		- CREATE TABLE Student：表示创建一个名为Student的关系表（二维表）
		- Sno CHAR(9) PRIMARY KEY：表示创建一个列名为Sno的列，其中数据类型为CHAR，最长占9个字符，PRIMARY KEY表示这列为主属性（主键/主码）
		- Sname CHAR(20) UNIQUE：表示创建一列名为Sname的列，占20个字符，UNIQUE表示不能为空
		- Sage SMALLINT：表示创建一列名为Sage的列，其中数据类型为SMALLINT
- 例：建立一个“课程”表Course
	- 语句如下：
		- CREATE TABLE Course</br>(Cno CHAR(4) PRIMARY KEY,</br>Cname CHAR(40),</br>Cpno CHAR(4),/\*先导棵\*/</br>Ccredit SMALLINT,</br>FOREIGN KEY (Cpno) REFERENCES Course(Cno));
	- 说明：
		- FOREIGN KEY (Cpno) REFERENCES Course(Cno)：这条语句表示Cpno被设置为外键，它所参照的表是Course这张表的Cno列
- 例：建立一个“学生选课”表SC
	- 语句如下：
		- CREATE TABLE SC</br>(Sno CHAR(9),</br>Cno CHAR(4),</br>Grade SMALLINT,</br>PRIMARY KEY(Sno,Cno),</br>FOREIGN KEY (Sno) REFERENCES Student(Sno),</br>FOREIGN KEY(Cno) REFERENCES Course(Cno));
	- 说明：
		- 由于这张表它需要利用Sno列和Cno列组成联合主键，因此它的定义需要在表级上进行主键定义

## <font color = "886600">创建基本表</font>
（看这小节前建议先看[[模式与表的关系]]这篇文章）
以定义一个学生-课程模式S-T为例：
- 创建表时给出模式名
	- 语句如下：
		- CREATE TABLE "S-T" .Student(……);
		- CREATE TABLE "S-T" .Course(……);
		- CREATE TABLE "S-T" .SC(……);
	- 说明：这种语句可以在创建表的同时将这张表放在"S-T"模式下。括号中的语句和<font color = "CC6600">「基本表的定义」</font>没有差别
- 在创建模式的时候同时创建表
	- 语句如下：
		- CREATE SCHEMA TEST AUTHORIZATION ZHAN</br>CREATE TABLE TAB1 </br>(COL1 SMALLINT,</br>COL2 INT,</br>COL3 CHAR(20),</br>COL4 NUMERIC(10,3),</br>COL5 DECIMAL(5,2));
- 设置所属模式，则在创建表明中不必给出模式名
	- 语句如下：
		- SET search_path TO "S-T",PUBLC;</br>CREATE TABLE Student(……);
	- 执行结果：建立了S-T.Student基本表

## <font color = "886600">修改基本表</font>
- 语句如下：
	- ALTER TABLE <表名></br>\[ADD\[COLUMN] <新列名> <数据类型> \[完整性约束]]</br>\[ADD <表级完整性约束>]</br>\[ADD<列级完整性约束> (列名)]</br>\[DROP \[COLUMN] <列名> \[CASCADE|RESTRICT]]</br>\[DROP CONSTRAINT <完整性约束名> \[RESTRICT|RESTRICT]]</br>\[ALTER COLUMN <列名><数据类型>];
		- 说明：
			- ADD\[COLUMN]：表示新增一列
			- Add <表级完整性约束>：表示添加一个完整性约束
			- DROP \[COLUMN]：表示删除一列
				- CASCADE级联删除：如果有一张表B参照了表A，那么当表A中的某一列被删除时，那么B表中和A表有关的列也要删除
				- RESTRICT限制删除：如果有一张表B参照了表A，那么当你删除A中的某一列时会不允许你删除
			- DROP CONSTRAINT：表示删除一个指定的完整性约束
			- ALTER COLUMN子句用于修改原有的列定义，包括列名和数据类型
- 例：向Student表增加“入学时间”列，其数据类型为日期型
	- 语句如下：
		- ALTER TABLE Student </br>ADD S_entrance DATE;
	- 注意：不论基本表中原来是否有数据，新增加的列一律为空值，所以我们新增的列不能为主属性（主码/主键）
- 例：将年龄的数据类型由字符型（假设原来的数据类型时字符型）改为整数
	- 语句如下：
		- ALTER TABLE Student ALTER COLUMN Sage INT;
- 例：增加课程名称必须取唯一值的约束条件
	- 语句如下：
		- ALTER TABLE Course ADD UNIQUE (Cname);

## <font color = "886600">删除基本表</font>
- 语句格式如下：
	- DROP TABLE <表名> \[RESTRICT|CASCADE];
- 例：删除Student表：
	- 语句如下：
		- DROP TABLE Student CASCADE;
	- 说明：
		- RESTRICT：限制删除，被删除的基本表不能被其他表依赖。如果存在依赖该表的对象，则删除失败
		- CASCADE：级联删除，删除该表时如果有其他的表依赖该表，那么依赖表也会被删除
		- 基本表被删除，数据被删除，表上建立的索引、视图、触发器等一般也将被删除
- 例：删除Student表，若表上建有视图，选择<font color = "CC6600">「RESTRICT」</font>时表不能被删除，选择<font color = "CC6600">「CASCADE」</font>时可以删除表，视图也会被自动删除
	- 语句如下：
		- CREATE VIEW IS_Student</br>AS</br>SELECT Sno.Sname,Sage</br>FROM Student</br>WHERE Sdept='IS';</br>DROP TABLE Student RESTRICT(DROP TABLE Student CASCADE);
	- 说明：
		- 如果我们选择RESTRICT删除，那么他就会报错
		- 如果选择CASCADE删除，则会连同视图一起删除