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
3. <a id = "隔离性">隔离性（Isolation）</a>：多个事务并发执行时，一个事务的执行不应该影响其他事务的执行。隔离性的隔离级别又分为以下几点：
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
		connection.Open();
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

			var p1 = new SqlParameter("Title",SqlDbType.NVarChar,50);
			var p2 = new SqlParameter("Publisher",SqlDbType.NVarChar,50);
			var p3 = new SqlParameter("Isbn",SqlDbType.NVarChar,20);
			var p4 = new SqlParameter("ReleaseDate",SqlDbType.Date);
			command.Parameters.AddRange(new sqlParameter[]{p1,p2,p3,p4});

			command.Parameters["Title"].Value = "……";
			command.Parameters["Publisher"].Value = "……";
			command.Parameters["Isbn"].Value = "42-08154711";
			command.Parameters["ReleaseDate"].Value = "……";

			int id = command.ExecuteScalar();
			Console.WriteLine($"record added with id:{id}");

			command.Parameters["Title"].Value = "…………";
			command.Parameters["Publisher"].Value = "…………";
			command.Parameters["Isbn"].Value = "42-08154711";
			command.Parameters["ReleaseDate"].Value = "…………";

			int id = command.ExecuteScalar();
			Console.WriteLine($"record added with id:{id}");
			
			tx.Commit();
		}
		catch(Exception ex)
		{
			Console.WriteLine($"error {ex.Message},rolling back");
			tx.Rollback();
		}
	}
}
```
- connection.Open()：首先使用connection.Open()打开连接
- connection.BeginTransaction()：利用BeginTransaction方法开始事务
- Transaction = tx：告诉command这是在执行一个事务而不是普通的sql语句
- p1 = new SqlParameter && command.Parameters[""]：定义和填充参数
- tx.Commit：提交事务
- tx.Rollback：事务提交失败，开始回滚
在上述代码示例中，由于两次的Isbn值相同（设置了唯一性），因此该事务会执行失败而执行回滚操作。<font color = "BA8448">【需要注意的是，如果在断点调试的模式下执行事务，那么事务有可能会因为断点激活的时间太长而导致中断，因为事务超时了。】</font>

## <font color = "886600">ADO.Net中的隔离级别</font>
在ADO.Net中除了上述<a href = "#隔离性">隔离性</a>中提到的四个隔离级别以外，ADO.Net还额外提供了三种隔离级别，如下所示：
- <font color = "CC6600">「快照隔离（Snapshot）」</font>：Snapshot用于对实际的数据进行快照。在复制修改的记录时，这个级别会减少锁定。这样，其他事务仍可以读取旧数据，而不需要等待解锁。这个隔离级别可以有效防止<font color = "CC6600">「脏读」</font><font color = "CC6600">「不可重复读」</font><font color = "CC6600">「幻读」</font>，同时不需要对读取的数据进行行锁定，从而提高并发性能
	- 该隔离级别是Sql Server 2005引入的一个隔离级别，它通过存储数据变更的版本来工作。在快照隔离级别中，读取操作会读取到事务开始之前的旧数据。例如：如果数据a在事务T1中被修改为b，而与此同时事务T2的事务隔离级别被设置为<font color = "CC6600">「快照隔离」</font>，那么T2读取到的数据依然是a
- <font color = "CC6600">「未指定（Unspecified）」：</font>Unspecified表示，提供程序使用另一个隔离级别值，该值不同于IsolationLevel枚举定义的值（一般不推荐）
- <font color = "CC6600">「混乱隔离（Chaos）」：</font>Chaos类似于ReadUncommitted，与Readuncommitted不同的是，它不能锁定更新的记录
下表表示了最常用的事务隔离级别可能导致的问题：

| 隔离级别 | 脏读 | 不可重复读 | 幻读 |
|:------:|:----:|:--------:|:---:|
| ReadUncommitted| Y | Y | Y |
| ReadCommitted | N | Y | Y |
| RepeatableRead | N | N | Y |
| Serializable | N | N | N |

下面将分别介绍如何为<font color = "CC6600">「SqoConnection」</font>、<font color = "CC6600">「CommittableTransaction」</font>和<font color = "CC6600">「TransactionScope」</font>设置隔离级别：
- SqlConnection：SqlTransaction transaction = connection.BeginTransaction(IsolationLevel.ReadCommitted);
- CommittableTransaction：var tx = new CommittableTransaction(IsolationLevel.ReadCommitted);
- TransactionOptions options = new TransactionOptions();
  TransactionScope scope = new TransactionScope(TransactionScopeOption.Required,options);
关于<font color = "CC6600">「CommittableTransaction」</font>和<font color = "CC6600">「TransactionScope」</font>的更多介绍可以看下文
# 事务和System.Transactions
<strong>在命名空间System.Transactions下有许多类可以供我们使用，而在其中<font color = "CC6600">「Transaction」</font>、<font color = "CC6600">「CommittableTransaction」</font>和<font color = "CC6600">「TransactionScope」</font>尤为重要</strong>
- Transaction类：它是所有事务类型的抽象基类，它提供了控制事务和管理事务的基础设施。可以用于直接访问环境事务，提供关于事务的信息，以及在发生故障时启动回滚（<font color = "BA8448">【Transaction类不能用来直接提交事务】）</font>
- CommittableTransaction：该类继承自Transaction，它是Transaction类的具体实现，它提供了提交事务的能力。
- TransactionScope：TransactionScope类提供了一种简化的事务模型，它自动管理事务的生命周期，并为代码块提供了一个事务上下文环境。
## <font color = "886600">Transaction类</font>
<strong>Transaction类可以用于直接访问环境事务，提供关于事务的信息，以及在发生故障时启动回滚，但是它不能用来提交事务</strong>。下表描述了Transaction类的成员：

| Transaction类成员 | 说明 |
|:----------------:|:---:|
| Current | Current是一个静态属性。如果存在环境事务，则Transaction.Current返回该事务 |
| IsolationLevel | IsolationLevel属性返回一个IsolationLevel类型的对象。IsolationLevel是一个枚举，它定义了其他事务对某事物的临时结果的访问权限（也就是获取隔离级别） |
| TransactionInformation | TransactionInformation属性返回TransactionInformation对象，它提供了关于事务当前状态、创建事务的时间和事务标识符的信息 |
| Rollback | 使用Rollback方法，可以中止事务，并撤销事务的所有部分  |
| DependentClone | 使用DependentClone方法，可以创建一个依赖当前事务的事务 |
| TransactionCompleted | TransactionCompleted是在事务完成时触发的事件——要么成功，要么失败。使用TransactionCompletedEventHandler类型的事件处理程序对象，可以访问Transaction对象并读取其状态 |

## <font color = "886600">可提交的事务（CommittableTransaction类）</font>
<strong>我们已经知道使用Transaction类无法提交事务，而在System.Transactions命名空间下唯一支持提交事务的类是<font color = "CC6600">「CommittableTransaction」</font></strong>
如下代码示例：
```C#
static async Task CommittableTransactionAsync()
{
	var tx = new CommittableTransaction();
	DisplayTransactionInformation("TX create",tx.TransactionInformation);

	try
	{
		var b = new Book
		{
			Title = "A Dog in The House",
			Publisher = "Pet Show",
			Isbn = RandomIsbn(),
			ReleaseDate = new DateTime(2018,11,24)
		};
		var data = new BookData();
		await data.AddBookAsync(b,tx);

		if(AbortTx())
			throw new ApplicationException("transaction abort by the user");
		tx.Commit();
	}
	catch
	{
		Console.WriteLine(ex.Message);
		Console.WriteLine();
		tx.Rollback();
	}
}
```
- var tx = new CommittableTransaction()：用于创建CommittableTransaction对象，该对象提供了提交事务、中止事务的操作
- tx.Commit()：用于提交事务
- tx.rollback()：如果事务执行失败则进行回滚

### <font color = "AA7700">依赖事务</font>
<strong>对于依赖事务，可以在多个任务或线程之间影响一个事务。依赖事务依赖于另一个事务，并影响事务的结果</strong>，请看下方示例代码：
```C#
static void DependentTransactions()
{
	async Task UsingDependentTransactionAsync(DependentTransaction dtx)
	{
		dtx.TransactionCompleted +=(sender,e)=>DisplayTransactionInformation("Depdendent TX completed",e.Transaction.TransactionInformation);

		DisplayTransactionInformation("Dependent TX",dtx.TransactionInformation);

		await Task.Delay(2000);

		dtx.Complete();//表示完成依赖事务
		DisplayTransactionInformation("Dependent TX send complete",dtx.TransactionInformation);
	}

	var tx = new CommittableTransaction();
	DisplayTransactionInformation("Root Tx created",tx.TransactionInformation);
	
	try
	{
		DependentTransaction depTx = tx.DependentClone(DependentCloneOption.BlockCommitUntilComplete);

		Task t1 = Task.Run(()=>UsingDependentTransactionAsync(depTx));

		if(AbortTx())
			throw new ApplicationException("……");
		
		tx.Commit();
	}
	catch
	{
		Console.WriteLine(ex.Message);
		tx.Rollback();
	}

	DisplayTransactionInformation("TX completed",tx.TransactionInformation);
}
```
- 在上述代码示例中，我们首先创建了一个基于任务的异步方法，以便在后序代码中能够使用它
- 之后我们使用CommittableTransaction创建一个根事务tx
- 我们接着使用tx的DependentClone方法创建一个依赖事务depTx
	- 我们在使用DependentClone方法创建依赖事务时，需要给DependentClone方法传递一个DependentCloneOption类型的参数，该参数是一个枚举，有两个值：<font color = "CC6600">「BlockCompleteUntilComplete」</font>和<font color = "CC6600">「RollbackIfNotComplete」</font>
		- BlockCompleteUntilComplete：如果将DependentCloneOption设置为该参数，那么tx.Commit就会等待，直到所有的依赖事务都完成为止
		- RollbackIfNotComplete：设置该参数，则如果依赖事务没有在根事务提交事务之前调用Complete方法，则事务将会被中止
- 我们将创建好的依赖事务depTx传递给UsingDependentTransactionAsync方法。

## <font color = "886600">环境事务</font>
<strong>也就是定义一个代码块，这个代码块是事务性代码，里面包含的各种操作都被视为事务</strong>
- 对于环境事务，我们不需要手动获取与事务的连接；这是由支持环境事务的资源自动完成的
- 环境事务与当前线程关联。可以使用静态属性Transaction.Current获取并设置环境事务
# 参考文献：
- .Net开发经典名著——《C#高级编程（第11版） C#7 & .Net Core 2.0》
- CommandBehavior枚举：[https://learn.microsoft.com/zh-cn/dotnet/api/system.data.commandbehavior?view=net-7.0]
- ExecuteScalar()方法：[https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.data.sqlclient.sqlcommand.executescalar?view=sqlclient-dotnet-standard-5.1]
- ExecuteNonQuer()方法：[https://learn.microsoft.com/zh-cn/dotnet/api/system.data.common.dbcommand.executenonquery?view=net-7.0]
- SqlCommand类：[https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.data.sqlclient.sqlcommand?view=sqlclient-dotnet-standard-5.1]
- CommandType：[https://learn.microsoft.com/zh-cn/dotnet/api/system.data.commandtype?view=net-7.0]
- System.Transactions命名空间：[https://learn.microsoft.com/zh-cn/dotnet/api/system.transactions?view=net-7.0]
- Transaction类：[https://learn.microsoft.com/zh-cn/dotnet/api/system.transactions.transaction?view=net-7.0]
- CommittableTransaction类：[https://learn.microsoft.com/zh-cn/dotnet/api/system.transactions.committabletransaction?view=net-7.0]
- TransactionScope类：[https://learn.microsoft.com/zh-cn/dotnet/api/system.transactions.transactionscope?view=net-7.0]
