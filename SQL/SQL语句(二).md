---
title: SQL语句(二)
date: 2023-11-07 19:49:36
top_image: https://raw.githubusercontent.com/Altholia/CodeNotesPicGo/main/202311052004558.jpeg
cover: https://raw.githubusercontent.com/Altholia/CodeNotesPicGo/main/202311052004558.jpeg
tags: 
- 基本表
- 模式
categories: 
- [数据库系统概论,SQL教程]
---
# 数据定义
<strong>SQL的数据定义功能有这几个方面：<font color = "CC6600">「模式定义」</font><font color = "CC6600">「表定义」</font><font color = "CC6600">「视图」</font>以及<font color = "CC6600">「索引的定义」</font>。有关它们的操作语句，如下图所示：</strong>

| 操作对象 | 创建 | 删除 | 修改 |
|:------:|:----:|:---:|:----:|
| 模式 | CREATE SCHEMA | DROP SCHEMA |
| 表 | CREATE TABLE | DROP TABLE | ALTER TABLE |
| 视图 | CREATE VIEW | DROP VIEW |
| 索引 | CREATE INDEX | DROP INDEX | ALTER INDEX |
## <font color = "886600">SQL中的数据类型</font>
| 数据类型 | 含义 |
|:-------:|:----:|
| CHAR(n) | 长度为n的定长字符串 |
| VARCHAR(n) | 最大长度为n的变长字符串 |
| INT | 长整数（也可以写作INTEGER）|
| SMALLINT | 短整数 |
|NUMERIC(p,d) | 定点数，由p位数字（不包括符号、小数点）组成，小数点后面有d位数字 |
| REAL | 取决于机器精度的浮点数 |
| Double Precision | 取决于机器精度的双精度浮点数 |
| FLOAT(n) | 浮点数，精度至少位n位数字 |
| DATE | 日期，包含年、月、日，格式为YYYY-MM-DD |
| TIME | 时间，包含一日的时、分、秒，格式为HH:MM:SS |
- <font color = "CC6600">「CHAR」</font>与<font color = "CC6600">「VARCHAR」</font>最大的不同就是CHAR是定长的，也就是说，如果你存入一个字符，那么存储字符的这个空间长度也是n；而如果使用VARCHAR，如果你存入一个字符，那么保存它的空间就会从n个空间缩减到1，也就是说存多少用多少
- <font color = "CC6600">「NUMERI(p,d)」</font>：这个表示整数为p位，小数为d位
- <font color = "CC6600">「FLOAT」</font>：这个表示整数和小数加起来的精度为n为，不管你的整数和小数是几精度
## <font color = "886600">模式的定义与删除</font>
- <strong>模式（Schema）是一个结构化的框架，用来描述在数据库中对象的组织方式。这些对象可以包括表（Table）、视图（View）、索引（Index）、存储过程（Stored Procedure）、序列（Sequence）、包（Package）以及触发器（Trigger）等</strong>
- <strong>每个数据库至少包含一个模式（Schema）。模式可以用来对数据库对象进行逻辑分组，这样可以更有效地管理对象的访问权限和安全性。模式也可以帮助避免名称的冲突，因为不同的模式下可以有相同的表名或其他对象</strong>
- <strong>模式的所有权是由数据库用户或角色确定的。通常情况下，一个模式的所有者可以对其内的对象执行任何操作，并可以授予其他用户对这些对象的访问权限</strong>
  - 小白解释一：假设我现在有一个大型场地用来管理车辆，这个场地中有许多个仓库，每个仓库用来管理不同品牌的车辆，然后为这些仓库安排不同的管理员，这样就只有相应的管理员才能进入这个仓库中。在这个例子中，场地就相当“database"，仓库相当于”schema“
  - 小白解释二：模式包含在数据库中，相当于数据库中包含一个小的数据库。例如在“master”数据库中，我们可以创建一个“Test”名称的Schema，并将权限给“arturia”用户，然后我们在“Test”下创建一个表Employees，这样这个表只有“arturia”用户才可以访问和修改别的用户无法访问，这样就可以保证安全性，并且还有助于管理。
  - ![image-20231107184521569](https://raw.githubusercontent.com/Altholia/CodeNotesPicGo/main/202311071845687.png)
### <font color = "AA7700">创建</font>
- <strong>创建格式如下：<font color = "CC6600">「CREATE SCHEMA &lt;模式名&gt; AUTHORIZATION &lt;用户名&gt;」</font></strong>
	- CREATE SCHEMA <模式名>：这一段表示创建一个模式名为”<模式名>"的Schema
	- AUTHORIZATION <用户名>：这一段表示将这个Schema的权限给“<用户名>"这个用户
  - 如果没有指定模式名，则<模式名>默认为<用户名>
	- 在CREATE SCHEMA中可以接受<font color = "CC6600">「CREATE TABLE」</font><font color = "CC6600">「CREATE VIEW」</font>以及<font color = "CC6600">「CREATE GRANT」</font>子句
	  - <font color = "CC6600">「CREATE SCHEMA &lt;模式名&gt; AUTHORIZATION &lt;用户名&gt; [&lt;表定义子句&gt; | &lt;视图定义子句&gt; | &lt;授权定义子句&gt;];」</font>
      - ```sql
        CREATE SCHEMA Test AUTHORIZATION Arturia
        	CREATE TABLE Table1;
      - 执行<font color = "CC6600">「CREATE SCHEMA」</font>语句必须拥有DBA权限
  - 问题：当我使用sa登录名登录时，它下面有一个master数据库，然后我为该数据库创建一个用户arturia（除了该用户以外还有另外两个用户），并且创建一个模式Test并将该模式的权限给予arturia，然后我为这个数据库创建一个表arturia.emply。现在在我的应用程序中使用连接字符串连接该数据库。请问我该如何保证只有访问和修改emply表的权限呢？
    - 答：你需要保证你的sa登录名成功映射到了arturia用户上，这样当你使用sa登录名登录时就映射到了arturia用户上，这样你就可以保证只拥有emply表的访问和修改权限，具体的映射SQL语句是：<font color = "CC6600">「USE master;
      CREATE USER altria FOR LOGIN Arturia;」</font>。然后赋予权限：<font color = "CC6600">「GRANT SELECT, INSERT, UPDATE, DELETE ON SCHEMA::Test TO altria;」</font>
### <font color = "AA7700">删除</font>
- 删除语句格式如下：<font color = "CC6600">「DROP SCHEMA &lt;模式名&gt; &lt;CASCADE | RESTRICT&gt;」</font>
  - <font color = "CC6600">「CASCADE」</font>和<font color = "CC6600">「RESTRICT」</font>必须二选一
    - CASCADE（级联）：级联删除，删除模式的同时会把该模式下的所有数据库对象（例如TABLE、VIEW和INDEX等）全部删除
    - RESTRICT（限制）：如果该模式中定义了下属的数据库对象（如TABLE、VIEW和INDEX等），则拒绝该删除语句的执行。只有当该模式下的没有任何下属的对象时才能执行
## <font color = "886600">基本表的定义、删除与修改</font>
### <font color = "AA7700">基本表的定义</font>
<strong>语句格式如下：</strong>
- <font color = "CC6600">CREATE TABLE &lt;表名&gt; </font>
  <font color = "CC6600">(&lt;列名&gt; &lt;数据类型&gt; [&lt;列级完整性约束条件&gt;]</font>
  <font color = "CC6600">[,&lt;列名&gt; &lt;数据类型&gt; [&lt;列级完整性约束条件&gt;]]</font>
  <font color = "CC6600">[,&lt;表级完整性约束条件&gt;]);</font>
  - []：这个符号表示里面的内容可有可无
  - CREATE TABLE：这个是关键字，表示创建一个表
  - <表名>：这个表名也是必须要有的，是你所需创建表的名字。例如：CREATE TABLE Student
  - ()：小括号里用于定义列名、列的数据类型（如是int、chat还是float）、列的完整性约束条件。每次定义完一列之后如果有下一列则用","分隔接着定义下一列
  - 最后用";"结束定义
- 例1：
  ```sql
  CREATE TABLE Student
  	(Sno CHAR(9) PRIMARY KEY,
      Sname CHAR(20) UNIQUE,
      SSex CHAR(2),
      Sdept CHAR(20));
  ```
  - 在上述示例中，我们创建了一个名为“Student"的表，其中将Sno作为该表的主键（列级完整性约束）并且设置Sname为唯一值
  - PRIMARY KEY：表示将该列（Sno）设置为主键
  - UNIQUE：表示该列的值不能重复
  - CHAR(n)：表示该列的数据类型为CHAR类型，而n表示占几个字符（<font color = "CC6600">「需要注意的是，一个汉字占两个字符」</font>）
- 例2：
  ```sql
  CREATE TABLE Course
  	(Cno CHAR(4) PRIMARY KEY,
      Cname CHAR(40),
      Cpno CHAR(4),
      Ccredit SMALLINT,
      FOREIGN KEY (Cpno) REFERENCES Course(Cno));
  ```
  - 上述代码中的最后一行表示：将Cpno属性设置为外键，它所参照的是Course表中的Cno属性（<font color = "CC6600">「需要注意的是，外键所参照的属性A，这个属性A在自己的表中必须是主键，这样才能成功设置外键」</font>）
  - 如果要将某列设置为外键，则需要在<font color = "CC6600">「表级完整性约束」</font>中进行定义，不能在列级完整性约束中定义
- 例3：
  ```sql
  CREATE TABLE SC
  	(Sno CHAR(9),
      Cno CHAR(4),
      Grade SMALLINT,
      PRIMARY KEY(Sno,Cno),
      FOREIGN KEY (Sno) REFERENCES Student(Sno),
      FOREIGN KEY (Cno) REFERENCES Course(Cno));
  ```
  由于SC这个表是由学生的学号（Sno）、课程号（Cno）以及成绩（Grade）所组成的，而要确定某一元组需要Sno+Cno共同确定时才能唯一确定某一元组，因此Sno+Cno组成联合组建，因此这需要在<font color = "CC6600">「表级完整性约束」</font>中进行定义
- 什么情况下才需要在<font color = "CC6600">「表级完整性约束」</font>中进行定义
  - 当完整性约束涉及两个或多个列时
  - 设置外键时

  
  
  
