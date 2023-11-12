---
title: ADO.Net和事务
date: 2023-11-07 22:14:33
top_image: https://raw.githubusercontent.com/Altholia/CodeNotesPicGo/main/202311082024112.png
cover: https://raw.githubusercontent.com/Altholia/CodeNotesPicGo/main/202311082023179.png
tags:
- [ADO.Net]
- [事务]
- [存储过程]
categories: 
- [C#]
- [.Net]
---
# 概述
<strong>ADO.Net属于Micorosrt.Net框架的一部分，用于在应用程序和数据库之间建立数据连接。它为数据访问提供了一组组件，这些组件使得.Net应用程序可以高效地与各种关系型数据进行交互，包括数据的查询、检、插入、更新和删除等操作</strong>
# 与数据库进行连接
<strong>为了访问数据库，我们可以使用<font color = "CC6600">「SqlConnection类」</font>与Sql Server进行连接</strong>
```C#
public static void OpenConnection()
{
    string connectionString = @"server=(localdb)\MSSQLLocalDB;integrated security=SSPI;database=Books;";
    
    var connection = new SqlConnection(connectionString);
    connection.Open();//打开数据库
    connection.Close();//关闭数据库
}
```
- server=(localdb)\MSSQLLocalDB：这个表示要连接到的数据库服务器。因为是本地所以是（localdb），而MSSQLLocalDB则是在安装SQL Server时创建的数据库服务器实例（一台计算机上，可以允许多个不同的数据库服务器实例）
- integrated security=SSPI：这个表示利用windows身份验证的方式连接到数据库
- database=Books：这个表示要连接到的数据库
# 命令
<strong>在ADO.Net中提供了三组命令组件，它们分别是：<font color = "CC6600">「ExecuteNonQuery()」</font><font color = "CC6600">「ExecuteReader()」</font>以及<font color = "CC6600">「ExecuteScalar()」</font></strong>
- ExecuteNonQuery()：执行命令，但不返回任何结果
- ExecuteReader()：执行命令，返回一个类型化的IDataReader
- ExecuteScalar()：执行命令，返回一个结果集中第一行第一列的值
## <font color = "886600">ExecuteNonQuery()方法</font>
<strong>因为这个方法不返回任何结果，因此这个方法一般用于<font color = "CC6600">「UPDATE」</font><font color = "CC6600">「INSERT」</font>或<font color = "CC6600">「DELETE」</font>语句，唯一的返回结果是受影响的记录个数。<font color = "CC6600">「但是，如果使用该方法调用一个带有输出参数的存储过程，则这个方法就有返回值」</font></strong>
```C#
public static void ExecuteNonQuery()
{
    try
    {
        using(var connection = new SqlConnection(GetConnectionString()))
        {
            string sql = "INSERT INTO [ProCSharp].[Books]" + 
                "([Title],[Publisher],[Isbn],[ReleaseDate])" + 
                "VALUES (@Title,@Publisher,@Isbn,@ReleaseDate)";
            
            var command = new SqlCommand(sql,connection);
            command.Parameters.AddWithValue("Title","Professional C# 7 and .Net Core 8.0");
            command.Parameters.AddWithValue("Publisher","Wrox Press");
            command.Parameters.AddWithValue("Isbn","978-1119449270");
            command.Parameters.AddWithValue("ReleaseDate",new DateTime(2023,11,8));
            
            connection.Open();
            int records = command.ExecuteNoneQuery();
            Console.WriteLine(records);
            connection.Close();
        }
    }
    catch(SqlException ex)
    {
        Console.WriteLine(ex.Message);
    }
}
```
- GetConnectionString()：用于获取连接字符串
- var command = new SqlCommand(sql,connection)：使用sql语句和SqlConnection进行初始化，用于后面执行<font color = "CC6600">「ExecuteNonQuery()方法」</font>
- command.Parameters.AddWithValue()：向sql字符串中的参数传递值（<font color = "CC6600">「注意，不要试图给SQL参数使用连接字符串，因为这很容易被SQL注入，我们应当始终使用上述代码块中的方式为SQL参数传递值」</font>）
- int records = command.ExecuteNoneQuery()：当执行这个方法时，它会返回1。因为我们使用sql语句插入了一条记录，受影响的记录为1，则返回1
## <font color = "886600">ExecuteScalar()方法</font>
<strong>执行查询，并返回查询所返回的结果集中的第一行的第一列。忽略其他列或行</strong>
- 结果集：例如我们需要从Student(Name,Sex,Age,Sno)中查询Name、Sex和Age，则返回的结果集就是（这里我们假设Student关系表中只有三行元组）：
  | Name | Sex  | Age  |
  | :--: | :--: | :--: |
  | 张三 |  男  |  24  |
  | 李四 |  男  |  25  |
  | 小红 |  女  |  21  |
- 对于上述的结果集，ExecuteScalar()方法只会返回“张三”这个结果（即：第一行第一列，并忽略其他行和列）
- 所以此方法比较适用于返回结果只有1的情况
## <font color = "886600">ExecuteReader()方法</font>
<strong>使用该返回会返回DataReader对象，我们可以使用这个对象遍历结果集中的所有元组</strong>
```C#
public static void ExecuteReader(string title)
{
    var connection = new SqlConnection(GetConnectionstring());
    var command = new SqlCommand(sql,connection);
    command.Parameters.AddWithValue("Title","Professional C# 7 and .Net Core 8.0");
    
    connection.Open();
    using(SqlDataReader reader = command.ExecuteReader(CommandBehavior.CloseConnection))
    {
        while(reader.Read())
        {
            int id = (int)reader["Id"];
            string bookTitle = (string)reader["Title"];
            string publisher = (string)reader["publisher"];
            
            //int id = reader.GetInt32(0);
            //string bookTitle = reader.GetString(1);
            //string publisher = reader.GetString(2);
        }
    }
}
```
- 在上述代码块中，我们没有在代码的最后明确的调用“connection.Close()"方法，这是因为我们在创建SqlDataReader对象时传递了一个：CommandBehavior.CloseConnection，这个值在关闭读取器时，与其关联的connection也会关闭
  - commandBehavior是一个枚举值，它具有如下字段：
    - CloseConnection：执行命令时，关闭关联的DataReader对象时，关联的Connection对象也会关闭
    - Default：这表示不设置任何的CommandBehavior标志，因此ExecuteReader(CommandBehavior.Default)等同于ExecuteReader()
    - KeyInfo：查询返回列和主键的信息
    - SingleResult：使用该标志，ExecuteReader()方法只会返回一个结果集
    - 有关更多CommandBehavior的信息点[这里](https://learn.microsoft.com/zh-cn/dotnet/api/system.data.commandbehavior?view=net-7.0)查询
- 当我们调用方法返回一个sqlDataReader对象时，便可以使用Read()方法循环遍历结果集中的元组，直到下一步位置没有元组了Read()方法就会返回一个false
- 访问元组中的属性时，有三种方法，这里只提供了两种：
  - 第一种：也是最慢的一种，但她可以满足要求，并且相对于发出服务调用所需的时间相比，访问索引器所需的额外时间可以忽略不计
  - 第二种：Get\*\*\*方法是强类型的，因为它们返回所需的特定类型，如int，string和DateTime。<font color = "CC6600">「传递给这些方法的索引对应于SQL SELECT语句检索的列，因此即使数据库的结构有所变化，<strong>该索引也保持不变</strong>」</font>，另外需要注意的是如果从数据库中返回了一个null值，则Get\*\*\*方法会抛出异常
## <font color = "886600">调用存储过程</font>
<strong>用命令对象调用存储过程，就是<strong></strong>存储过程的名字，给过程中的每个参数添加参数定义，然后利用上述的三种方法调用命令</strong>
如下示例显示了如何调用存储过程：
```sql
CREATE PROCEDURE [ProCSharp].[GetBooksByPublisher]
	@publisher nvarchar(50)
AS
	SELECT [Id],[Title],[Publisher],[ReleaseDate] FROM [ProCSharp].[Books]
	WHERE [Publisher] = @publisher ORDER BY [ReleaseDate]
```
为了能够调用存储过程，我们需要将SqlCommand对象的CommandText设置为存储过程的名称，CommandType设置为CommandType.StoredProcedure，如下所示：
```C#
private static void StoredProcedure(string publisher)
{
	using(var connection = new SqlConnection(GetConnectionString()))
	{
		SqlCommand command = connection.CreateCommand();
		command.CommandText = "[ProCSharp].[GetBooksByPublisher]";
		command.CommandType = CommandType.StoredProcedure;

		command.Parameters.AddWithValue("@publisher",publisher);
		connection.Open();

		using(SqlDataReader reader = command.ExecuteReader())
		{
			while(reader.Read())
			{
				//………………
			}
		}
	}
}
```
- 我们首先将CommandText设置为存储过程的名字
- 接着将CommandType设置为StoredProcedure，这将会告诉command这是在调用一个存储过程而不是一个sql语句
	- CommandType是一个枚举类型，它具有以下值：
		- StoredProcedure：表示调用存储过程
		- TableDirect：表示返回指定表的所有行和列，但有多个表时，返回的结果就是所有表的联接(需要注意的是，TableDirect无法运用到SQL Server中)
		- Text：表示执行一个普通的sql语句
- 紧接着我们就利用Parameters.AddWithValue设置参数
- 后面的步骤就是调用那三种方法读取数据了

# 事务
<strong>数据库事务：是数据库管理系统执行过程中的一个逻辑单元，由一个有限的数据库操作序列组成。当事务被提交给了<font color = "CC6600">「数据库管理系统（DBMS）」</font>，则DBMS需要确保该事务中的所有操作都成功且结果被永久保存在数据库中，如果事务中有的操作没有成功完成，则事务中的所有操作都需要回滚，回到事务执行前的状态；同时，该事务对数据库或者其他事务的执行无影响，所有的事物都好像在独立的运行</strong>，则它具有以下四个特性：<font color = "CC6600">「Atomicity（原子性）」</font><font color = "CC6600">「Consistency（一致性）」</font><font color = "CC6600">「Isolation（隔离性）」</font><font color = "CC6600">「Durability（持久性）」</font>
1. 原子性（Atomicity）：事务作为一个整体被执行，包含在其中的对数据库的操作要么全部执行，要么都不执行
2. 一致性（Consistency）：事务应确保数据库的状态从一个一致状态转变为另一个一致状态（一致状态：数据库中的数据应满足完整性约束）。
3. 隔离性（Isolation）：多个事务并发执行时，一个事务的执行不应该影响其他事务的执行。隔离性的隔离级别又分为以下几点：
	- <font color = "CC6600">「读未提交（ReadUncommitted）」</font>：最低的隔离级别，允许事务看到其他事务尚未提交的更改。这可能会导致出现<font color = "CC6600">「脏读」</font>（脏读：即读到其他事务尚未提交的数据）
	- <font color = "CC6600">「读提交（ReadCommitted）」</font>：这个级别确保一个事务只能读取已经被其他事务提交的数据。这可以有效防止出现“脏读”现象。但仍然可能会遇到其他问题，如<font color = "CC6600">「不可重复读」</font>（同一个事物中，多次读取同一个数据，结果可能不一致）和<font color = "CC6600">「幻读」</font>（同一个事务中，多次查询返回的结果集不一致，例如新的行被插入或删除）
	- <font color = "CC6600">「可重复读（Repeatable Read）」</font>：这个级别确保在同一个事务中多次读取同一份数据时，看到的结果是一致的。这可以有效防止出现<font color = "CC6600">「脏读」</font>和<font color = "CC6600">「不可重复度」</font>，但是仍然有可能会遇到<font color = "CC6600">「幻读」</font>问题
	- <font color = "CC6600">「串行化（Serializable）」</font>：这是最高的隔离级别，它通过对涉及的所有行所有列都通过添加<font color = "CC6600">「锁」</font>来防止并发访问。这可以非常有效的防止<font color = "CC6600">「脏读」</font><font color = "CC6600">「不可重复读」</font>和<font color = "CC6600">「幻读」</font>，但代价就是性能开销最大，因为它会限制并发性
4. 持久性（Durability）：已被提交的事务对数据库的修改应该永久保存在数据库中

## <font color = "886600">在ADO.Net中使用事务</font>
如果想要在ADO.Net中使用事务，可以通过调用<font color = "CC6600">「SqlConnection」</font>的<font color = "CC6600">「BeginTransaction」</font>方法就可以开始事务。<font color = "BA8448">【事务总是与一个连接关联起来，不能在多个连接上创建事务】</font>，下方的代码表示了如何使用ADO.Net开始一个事务：
```C#
public static void TransactionSample()
{
	using(var connection = new SqlConnection(GetConnectionString()))
	{
		await connection.OpenAsyn();
		SqlTransaction tx = connection.BeginTransaction();
		
		//...
		try
		{
			string sql = "INSERT INTO [ProCSharp].[Books]"+
				"([Title],[Publisher],[Isbn],[ReleaseDate])"+
				"SELECT SCOPE IDENTITY()";

			var command = new SqlCommand
			{
				CommandText = sql,
				Connection = connection,
				Transaction = tx
			}
		}
	}
}
```
- connection.BeginTransaction：表示开启一个事务
- SCOPE IDENTITY：一个函数，用来返回最后插入记录的标识值或自动递增的值
- Transaction=tx：表示这个sql语句是用来执行一个事务而不是普通的sql语句


# 参考文献：
- .Net开发经典名著——《C#高级编程（第11版） C#7 & .Net Core 2.0》
- CommandBehavior枚举：[https://learn.microsoft.com/zh-cn/dotnet/api/system.data.commandbehavior?view=net-7.0]
- ExecuteScalar()方法：[https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.data.sqlclient.sqlcommand.executescalar?view=sqlclient-dotnet-standard-5.1]
- ExecuteNonQuer()方法：[https://learn.microsoft.com/zh-cn/dotnet/api/system.data.common.dbcommand.executenonquery?view=net-7.0]
- SqlCommand类：[https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.data.sqlclient.sqlcommand?view=sqlclient-dotnet-standard-5.1]
- CommandType：[https://learn.microsoft.com/zh-cn/dotnet/api/system.data.commandtype?view=net-7.0]
