---
title: UML类图的几大关系总结
date: 2024-01-20 19:52:16
description: 这篇博客详细讲解了UML类图中的七大关系：泛化、实现、关联、聚合、组合、依赖和内部类关系。文章通过定义每种关系的特点、展示示意图，并提供实例来直观地描述它们在对象和类的设计中如何相互作用，从而帮助读者理解各种关系在软件开发和系统设计中的应用。
categories: 
- [UML]
tags: 
- [C#]
- [UML类图]
top_image: https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202401202200191.png
cover: https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202401202200191.png
keywords: 
- 泛化关系
- 实现关系
- 关联关系
- 聚合关系
- 依赖关系
- 内部类
---

# 所用工具
1. draw.io
	- 版本：v22.1.21
	- 用途：绘图
	- 获取方式：<a href="https://www.drawio.com/">官网下载</a>
# UML中的几大关系
在UML类图中，常见的有以下几大关系：<font color = "CC6600">「泛化关系（Generalization）」</font>、<font color = "CC6600">「实现关系（Realization）」</font>、<font color = "CC6600">「关联关系（Association）」</font>、<font color = "CC6600">「聚合关系（Aggregation）」</font>、<font color = "CC6600">「组合关系（Composition）」</font>、<font color = "CC6600">「依赖关系（Dependency）」</font>以及<font color = "CC6600">「内部类关系（Inner）」</font>

下图是表示这七种关系的示意图：
![UML七大关系示意图.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202401202106815.png)


## <font color = "886600">泛化关系（Generalization）</font>
> <strong><font color = "#D35400">泛化关系：指的是一个类继承另外的一个类的方法和属性，并可以自己增加一些额外的功能，泛化是类与类或接口与接口之间常见的关系（其实泛化关系就是继承关系）</font></strong>

泛化关系的示意图如下：
![泛化关系示意图.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202401202023988.png)
- 菱形箭头指向被继承的类或接口

类与类之间的泛化关系示意图如下：
![类与类之间的泛化关系示意图.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202401202046303.png)

接口与接口之间的泛化关系示意图如下：
![接口与接口之间的泛化关系示意图.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202401202047393.png)

## <font color = "886600">实现关系</font>
> <strong><font color = "#D35400">实现关系：指的是一个类实现interface接口（可以实现多个接口）的功能，实现是类与接口之间最常见的关系</font></strong>

实现关系示意图如下：
![实现关系示意图.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202401202053219.png)
- 菱形箭头指向被实现的接口

类与接口之间的实现关系示意图如下：
![类与接口之间的实现关系示意图.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202401202051990.png)

## <font color = "886600">关联关系</font>
> <strong><font color = "#D35400">关联关系：关联关系强调的是拥有。它使一个类可以知道另一个类的属性和方法。关联关系可以是单向的，也可以是双向的</font></strong>

关联关系示意图如下：
![关联关系示意图.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202401202107479.png)
- 箭头那边代表被拥有

类与类之间的关联关系示意图如下：
![单向关联关系示意图.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202401202119091.png)

双向关联关系示意图如下：
![双向的关联关系示意图.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202401202124067.png)

## <font color = "886600">聚合关系</font>
> <strong><font color = "#D35400">聚合关系：聚合关系强调的是“整体—部分”之间的关系，且部分可以离开整体单独存在</font></strong>

聚合关系是关联关系的一种，是强的关联关系；关联和聚合在语法上无法区分，必须考察具体的逻辑关系

聚合关系示意图如下：
![聚合关系示意图.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202401202126250.png)
- 菱形那边代表整体，箭头那边代表部分

例如车与车轮、车与引擎之间的关系就是聚合关系，因为车是由引擎和车轮组成的，而引擎和车轮又可以单独存在并给其他车型使用，示意图如下：
![聚合关系示意图.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202401202133206.png)
- 数字代表数量，一个车只能有一个引擎和4个轮子

## <font color = "886600">组合关系</font>
> <strong><font color = "#D35400">组合关系：组合关系依旧是“整体-部分”之间的关系，但是部分不能离开整体单独存在</font></strong>

组合关系是关联关系的一种，是比聚合关系还要强的关系，它要求部分的生命周期与整体的生命周期一样（共存亡）

组合关系示意图如下：
![组合关系示意图.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202401202137898.png)
- 实体菱形指向整体，箭头指向部分

例如人是由心脏构成的，但是心脏不可以脱离人单独存活（不考虑科技），示意图如下：
![组合关系示意图.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202401202143400.png)

## <font color = "886600">依赖关系</font>
> <strong><font color = "#D35400">依赖关系：是一种使用关系，即一个类的实现需要另一个类的协助，所以要尽量不使用双向的互相依赖</font></strong>

依赖关系示意图如下：
![依赖关系示意图.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202401202145824.png)
- 箭头指向被使用的类

例如转换类的Json转换功能需要JsonSerializer类的协助，示意图如下：
![依赖关系示意图.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202401202148673.png)

## <font color = "886600">内部类关系</font>
> <strong><font color = "#D35400">内部类：内部类关系强调的是嵌套关系，当一个类包含另一个类时，它们之间的关系就是内部类关系</font></strong>

内部类关系示意图如下：
![内部类示意图.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202401202154692.png)
- 加号指向外部类，箭头指向内部类

外部类与内部类之间的内部类关系示意图如下：
![内部类关系示意图.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202401202158448.png)

# 参考文献
- 【文章】[UML一一 类图关系 (泛化、实现、依赖、关联、聚合、组合)\_uml类图关系-CSDN博客](https://blog.csdn.net/m0_37989980/article/details/104470064)
- 【文章】[用Astah画UML表示内部类的画法\_uml nest-CSDN博客](https://blog.csdn.net/zjc_jiancheng/article/details/104769130)
- 【文章】[UML类图几种关系的总结 - 刘小吉 - 博客园](https://www.cnblogs.com/liuxiaoji/p/4704294.html)
- 【文章】[关联、聚合、组合的区别 - 知乎](https://zhuanlan.zhihu.com/p/359672087) 
- 【文章】[UML:类图关系总结 - 知乎](https://zhuanlan.zhihu.com/p/93289356)