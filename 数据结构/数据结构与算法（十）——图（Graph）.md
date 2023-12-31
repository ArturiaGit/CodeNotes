---
title: 数据结构与算法（十）——图（Graph）
date: 2023-12-31 09:43:10
description: 
categories: 
tags: 
top_image: 
cover: 
keywords:
---
# 图的定义
<strong>图是由顶点的<font color = "CC6600">「有穷非空集合」</font>和顶点之间的边的集合组成，通常表示为G(V,E)。其中，G表示一个图，V表示顶点的集合，E是边的集合</strong>
- 在图中，我们将数据元素称为<font color = "CC6600">「顶点（Vertex）」</font>
- 图，不像[[数据结构与算法（五）——线性表|线性表(List)]]和[[数据结构与算法（六）——树（Tree）|树(Tree)]]那样具有线性关系和层次关系，在图中，任意两个顶点之间都可能存在关系，顶点之间的逻辑关系用边来表示，<font color = "BA8448">【边集可以为空】</font>
- <font color = "BA8448">【在图中，不允许没有顶点。在定义中，若V是顶点的集合，则强调了顶点集合V是“有穷非空集合”】</font>

# 图的分类
![图的分类.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202312311027737.png)
## <font color = "886600">无向图</font>
<strong>如果图中任意两个顶点之间的边都是无向边，那么这个图称为无向图</strong>
- 无向边：若顶点v<sub>i</sub>到顶点v<sub>j</sub>的边是没有方向的，则称这条边为<font color = "CC6600">「无向边」</font>，用无序偶对（v<sub>i</sub>,v<sub>j</sub>)表示
- <font color = "BA8448">【注意，无序偶对用“()”表示】</font>
![无向图.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202312311052278.png)
## <font color = "886600">有向图</font>
<strong>如果图中任意两个顶点之间的边都是有向边，那么这个图就是有向图</strong>
- 有向边：若顶点v<sub>i</sub>到顶点v<sub>j</sub>的边是有方向的，则称这条边为<font color = "CC6600">「有向边」</font>，也称为<font color = "CC6600">「弧」</font>。用有序偶对\<v<sub>i</sub>,v<sub>j</sub>>表示，其中，v<sub>i</sub>是<font color = "CC6600">「弧尾」</font>，v<sub>j</sub>是<font color = "CC6600">「弧头」</font>
- 如下图就是一个有向图，顶点A到顶点E的边就是有向边（弧），A是弧尾，E是弧头
![有向图.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202312311101931.png)
## <font color = "886600">无向完全图</font>
<strong>在无向图中，如果任意两个顶点之间都存在边，则称该图为无向完全图</strong>
- 在无向完全图中，每个顶点都要与其他（n-1）个顶点连接，因此我们得到每个顶点有（n-1）条边。由于图中共有（n）个这样的顶点，因此整个图貌似有（n×(n-1)）条边。然后，由于每条边在这个过程中被计算了两次，因此我们需要在全部边得基础上除以2得到不重复的边数，即：<font color = "BA8448">【含有n个顶点的无向完全图，它的边数为：$\frac{n(n-1)}{2}$】</font>
![无向完全边.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202312311121334.png)
## <font color = "886600">有向完全图</font>
<strong>在有向图中，如何任意两个顶点之间都存在方向相反的两条弧，则称该图为有向完全图</strong>
- 含有（n）个顶点的有向完全图，它具有（n(n-1)）条边。
![有向完全图.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202312311137155.png)
## <font color = "886600">网(Network)</font>
<strong>带权的图通常被称为网(Network)</strong>)
- 权：与图的边或弧相关的数叫做权(Weight)
- 下图就是一张带权的图，边上的权表示的意思是两地之间的距离
![Network.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202312311143844.png)
# 图的顶点与边的关系
- 无向图：
	- 对于无向图G=(V,{E})，如果两个顶点（v和v<sup>'</sup>）之间的边（v,v<sup>'</sup>）属于边集合E，则称顶点v和顶点v<sup>'</sup>互为邻接点(Adjacent)，边（v,v<sup>'</sup>）依附于顶点v和顶点v<sup>'</sup>，或者说边（v,v<sup>'</sup>）与顶点v和v<sup>'</sup>相关联。顶点v的<font color = "CC6600">「度(Degree)」</font>是和v相关联的边的数目，记为TD(V)
	- 边数是各个顶点度数的和的一半，记为：$e = \frac{1}{2} \sum_{i=1}^{n} TD(V_{i})$
- 有向图：


