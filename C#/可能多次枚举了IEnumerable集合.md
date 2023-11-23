---
title: 可能多次枚举了IEnumerable集合
date: 2023-11-23 15:21:25
description: 这篇文章深入探讨了在.NET开发环境中使用IEnumerable<T>接口时遇到的一个常见问题——可能的多次枚举警告。它详细说明了IEnumerable<T>的特性、使用场景，以及为何在特定操作如前序与中序遍历二叉树构造中可能触发这类警告。文章内还提供了有效的解决方案和相关的参考链接，帮助使用者理解和规避多次枚举导致的性能问题。
categories:
  - - C#
  - - .Net
tags: 
- C#
- IEnumerable<T>
top_image: https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202311231757974.png
cover: https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202311231757974.png
keywords:
  - IEnumerable<T>
  - IEnumerable
  - CA1851
  - 多次枚举
  - C#
---
# 工具和环境
- Visual Studio 2022 Preview（Enterprise）
  - 版本：17.9.0 Preview 1.0
- ReSharper
  - 版本：2023.2.2
- .NET
  - 版本：.NET 8.0

# 深入理解IEnumerable\<T>接口

在处理复杂的数据结构，如[[二叉树（Binary Tree）]]时，我们经常会遇到使用`IEnumerable<T>`接口的场景。`IEnumerable<T>`是.NET中一个非常强大的接口，它定义了一个标准，用于支持对一个非泛型集合的简单迭代。本质上，`IEnumerable<T>`接口包括一个重要的方法`GetEnumerator()`，它返回一个可以迭代集合的迭代器对象。

迭代器提供了一种访问集合元素的方式，使我们能够用如`foreach`循环和LINQ查询这样的结构遍历集合。`IEnumerable<T>`接口特别适合于只读和枚举操作，因为它不提供修改集合的方法。

# 遇到的问题

在实现一个根据用户输入的前序和中序遍历数据构造二叉树的功能时，我需要接收两个`IEnumerable<T>`类型的集合：`preOrder`和`inOrder`，同时确保这两个集合符合某些前提条件，否则可能会抛出异常。检查条件包括：

- 确认`preOrder`和`inOrder`的长度是否相等：`preOrder.Count() != inOrder.Count()`。
- 检查`preOrder`和`inOrder`是否为空或者是否不包含元素：`if(preOrder is null || !preOrder.Any())`。

然而，尽管上述检查在逻辑上是必需的，但我在IDE中收到了这样一个警告：“Possible multiple enumeration”，意味着可能会多次枚举`IEnumerable`。

![可能的多次枚举警告](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202311231517578.png)

# 警告的原因

<a href = "https://www.jetbrains.com/help/resharper/2023.2/PossibleMultipleEnumeration.html">ReSharper</a>的官方文档告诉我们如果在多个`foreach`语句中使用`IEnumerable<T>`集合，可能会导致集合被多次枚举。这在某些情况下不仅会增加不必要的计算负担，如果集合代表了某个数据库查询的结果，还可能因为数据库状态的变化而导致不一致的结果。

<a href = "https://learn.microsoft.com/zh-cn/dotnet/fundamentals/code-analysis/quality-rules/ca1851">Microsoft</a>的官方文档进一步解释了`IEnumerable<T>`的延迟执行特性，多次枚举不仅可能影响性能，还可能引发bug，特别是如果枚举操作成本较高或具有副作用时。

言而简之(雾)，`IEnumerable<T>`并不存储数据本身，而是存储了一个可以产生数据的查询（例如，如果GetNames返回一个IEnumerable\<string>类型的参数，`IEnumerable<string> names = GetNames()`，那么names存储的并不是实际的数据信息，而是对`GetNames()`方法的调用）。每次遍历`IEnumerable<T>`实际上就是在执行这个查询方法。为了避免多次调用这个查询方法，这就带来了一个问题：如何在确保只枚举一次的同时仍然进行必需的检查？

# 解决方案

解决这个问题的最简单方法是在检查之前，将`IEnumerable<T>`转换为列表或数组，这样就可以多次访问这些集合而不会多次执行查询。转换可以使用`.ToList()`或`.ToArray()`方法实现，这样就缓存了查询结果，而不是每次都重新执行查询。

```csharp
List<T> preOrderList = preOrder.ToList();
List<T> inOrderList = inOrder.ToList();
```

现在我们可以在这些列表上执行任意多的操作，而不会收到可能的多次枚举警告。

# 结论

这篇文章解释了在使用`IEnumerable<T>`接口时为什么会出现的CA1851问题，并提供了相应的解决方案

# 新问题
为什么我对names进行断点调试能看到确切的值？
```C#
internal class Program
{
    static void Main(string[] args)
    {
        IEnumerable<string> names = GetNames();

        Console.WriteLine(names);
        foreach (string name in names)
            Console.WriteLine(name);
    }

    static IEnumerable<string> GetNames() => new List<string>() { "张三", "李四", "王五", "钻石" };
}
```
![image.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202311231813338.png)
- 虽然`GetNames()`方法返回的类型是`IEnumerable<string>`类型，但是我们还需要关注到它背后的具体实现方法。在上述示例中，`GetNames()`方法返回一个初始化好的`List<string>`。因此，在这个方法中，它虽然返回的类型是`IEnumerable<string>`，但是它实际引用的是`List<string>`(如下图输出类型），因此我们能在`names`中查看到确切的值。（而且IDE为了方便开发者能够查看和检查变量内容，会在后台执行代码以获取这些值）
	- ![image.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202311231824755.png)

- 如果我们需要模拟<font color = "CC6600">「延迟查询」</font>，需要修改代码，即通过使用迭代器的方式进行模拟：
```C#
internal class Program
{
    static void Main(string[] args)
    {
        IEnumerable<string> names = GetNames();

        Console.WriteLine(names);
        foreach (string name in names)
            Console.WriteLine(name);
    }

    static IEnumerable<string> GetNames()
    {
        yield return "张三";
        yield return "李四";
        yield return "王老五";
        yield return "钻石";
    }
}
```
大家可以自己去执行下这段代码，可以很容易发现，每当我们执行`foreach`语句时，`names`都会执行一遍`GetNames()`方法
# 参考链接

- [可能多次枚举“IEnumerable”集合 - .NET | Microsoft Learn](https://learn.microsoft.com/zh-cn/dotnet/fundamentals/code-analysis/quality-rules/ca1851)
- [Code Inspection: Possible multiple enumeration of IEnumerable | ReSharper Documentation](https://www.jetbrains.com/help/resharper/2023.2/PossibleMultipleEnumeration.html)