---
title: SQL语句(五)
date: 2023-11-22 20:04:04
description: 
categories: 
tags: 
top_image: 
cover: 
keywords:
published: false
---
# 数据查询
- 语句格式如下：
	- SELECT \[ALL|DISTINCT] <目标列表达式></br>\[,<目标列表达式>]……</br>FROM <表名或视图名>\[,<表名或视图名]……</br>\[WHERE <条件表达式>]</br>\[GROUP BY <列名1>\[HAVING <条件表达式>]]</br>\[ORDER BY <列名2>\[ASC|DESC]];
- 说明：
	- SELECT：表示选择要显示哪些信息（例如姓名、学号）
	- FROM：表示要从哪张基本表（视图）查找这些信息
	- WHERE：表示条件
	- GROUP BY：表示分组。比如我需要查询一年级各班的整体平均成绩，那我们可以将一年一班、一年级二班、一年级三班进行分组查询，然后再将这些平均成绩加起来求整个年级的平均成绩
	- ORDER BY：表示排序，按哪列排序
- 注意：
	- 语句中的字母不分大小写
	- 语句中的“,;”等标点字符为英文状态下的半角符号
	- \[]中的内容不是语句必须的内容，只是为了实现某些需求时才添加

## <font color = "886600">单表查询</font>
<strong>功能：对一个表的内容进行查询</strong>
### <font color = "AA7700">选择表中的若干列</font>
- 选择表中的若干列
	- 功能：查询指定列
	- 语句格式：在SELECT后面指定列名，FROM后面跟列所在的表名
	- 例如：查询全体学生的学号和姓名
		- SELECT Sno,Sname </br>FROM Student;
	- 例如：查询全体学生的姓名、学号、所在系
		- SELECT Sname,Sno,Sdept </br>FROM Student;
- 查询全部列
	- 功能：选出表中所有的属性列
	- 语句格式：在SELECT关键字后面列出所有的列名或将<目标列名称>指点为\*
	- 例如：查询全体学生的详细记录
		- SELECT Sno,Sname,Ssex,Sage,Sdept </br>FROM Student;
		- SELECT \* FROM Student;
- 查询经过计算的值
	- 功能：选出表中指定的属性列，并经过计算后输出
	- 格式：SELECT子句的<目标列表达式>可以为：
		- 算术表达式
		- 字符串常量
		- 函数
		- 列别名
	- 例如：查询全体学生的姓名及其出生年份
		- 语句：SELECT Sname,2004-Sage FROM Student;
		- 说明：由于在学生表中没有存储学生的出生年份，因此我们需要利用当前的年份减去学生的年龄，即：2004-Sage来计算学生的出生年份
		- 输出结果如下：
			- ![image.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202311222042133.png)
	- 例如：查询全体学生的姓名、出生年份和所有系，要求用小写字母表示所有系名
		- 语句：SELECT Sname,'Year of Birth:',2014-Sage,LOWER(Sdept)</br>FROM Student;
		- 说明：
			- 'Year of Birth:'：由于该字符串在表中没有，因此它是什么那么查询出来的结果就是什么）
			- LOWER()：这是一个函数，用来将查询到结果转换为小写字母，转换为大写字母的是UPPER()函数
		- 输出结果如下图：
			- ![image.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202311222032189.png)
	- 例如：使用列别名改变查询结果的列标题
		- 语句：SELECT Sname NAME,'Year of Birth:' BIRTH 2004-Sage BIRTHDAY,LOWER(Sdept) DEPARTMENT FROM Student;
		- 说明：
			- Sname NAME：表示将NAME设置为Sname的别名，那么在查询出的结果集中就会显示NAME名称而不是Sname
			- 'Year of Birth:' BIRTH：表示将BIRTH设置成'Year of Birth:'的别名
			- 2004-Sage BIRTHDAY：表示将BIRTHDAY设置成2004-Sage的别名
			- LOWER(Sdept) DEPARTMENT：表示将DEPARTMENT设置成LOWER(Sdept)的别名
		- 输出结果如下图：
			- ![image.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202311222041394.png)
### <font color = "AA7700">选择表中的若干元组</font>
<strong>be continue……</strong>

