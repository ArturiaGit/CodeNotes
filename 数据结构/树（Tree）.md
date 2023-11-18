---
title: 树（Tree）
date: 2023-11-14 08:57:11
description: 这篇文章详细介绍了树这种数据结构的基本定义和特点，包括树的结点的分类和结点间的关系。文章还通过图表形式解释了树结构的层次、深度、有序树和森林等概念，并对比了树与线性表的结构差异。
categories:
  - - 数据结构
    - 树
tags:
  - 树
top_image: https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202311172013031.png
cover: https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202311172013031.png
keywords:
  - 树的定义
  - 树的抽象类型
---
# 树的定义
<strong>树（Tree）是n（n>=0）个结点的有限集</strong>
- n=0：被称为空树（EmptyTree）
- 在任意一棵非空树下：
	- 有且只有一个特定的结点被称为<font color = "CC6600">「根结点（root）」</font>
	- 当n>1时，其余结点可分为m（m>0）个<font color = "CC6600">「互不相交」</font>的有限集T<sub>1</sub>，T<sub>2</sub>，T<sub>3</sub>……T<sub>m</sub>，其中每一个有限集本身又是一棵子树，并且被称为<font color = "CC6600">「根的子树（SubTree）」</font>

# 树的结点
<strong>树的结点包含一个数据元素以及若干指向其子树的分支</strong>
## <font color = "886600">结点的分类</font>
- 度：一个结点有多少个直接子树就有多少个<font color = "CC6600">「度」</font>
- 叶结点：度为0的结点称为<font color = "CC6600">「叶结点（或终端结点）」</font>
- 分支结点：度不为0的结点被称为<font color = "CC6600">「分支结点」</font>
- 内部结点：除<font color = "CC6600">「根结点以外」</font>，分支结点也被称为<font color = "CC6600">「内部结点」</font>
- 树的度：树的度是树内部各个结点中最大的那个度（例如，A结点有4个度，B结点有3个度，C结点有6个度，则该树的度就是6）<font color = "BA8448">【需要注意的是，根结点不参与度的计算】</font>
![树.drawio.png](https://raw.githubusercontent.com/Altholia/CodeNotesPicGo/main/202311141010030.png)

## <font color = "886600">结点之间的关系</font>
- 孩子（Child）：结点的子树的根被称为<font color = "CC6600">「孩子（Child）」</font>
- 双亲（Parent）：“结点的子树的根被称为<font color = "CC6600">「孩子（Child）」</font>”，则该结点被称为<font color = "CC6600">「双亲（Parent）」</font>
- 兄弟（brother）：同一个双亲的结点互为<font color = "CC6600">「兄弟（brother）」</font>
- 如下图所示：A结点是B结点和C结点的双亲，所以B和C是互为兄弟
![树.drawio.png](https://raw.githubusercontent.com/Altholia/CodeNotesPicGo/main/202311141018157.png)

# 树的其他概念
- 层次（Level）：从根开始定义，根为第一层，根的孩子为第二层
- 堂兄弟：双亲在同一层的结点互为堂兄弟。
- 最大深度（Depth）：树中结点最大层次被称为树的<font color = "CC6600">「深度（Depth）」</font>或高度
![树.drawio.png](https://raw.githubusercontent.com/Altholia/CodeNotesPicGo/main/202311141034727.png)
- 有序树：如果将树中的结点的各子树看成从左到右是有次序的，不能互换的，则称该树为<font color = "CC6600">「有序树」</font>，否则就是<font color = "CC6600">「无序树」</font>
- 森林（Forest）：是m(m>=0)棵<font color = "CC6600">「互不相交」</font>的树的集合

# 树与线性表的结构对比
| 线性结构 | 树结构 |
|:-------:|:-----|
| 第一个数据元素：无前驱 | 根结点：无双亲，唯一 |
| 最后一个数据元素：无后继 | 叶结点：无孩子，可以多个 |
| 中间元素：一个前驱和一个后继 | 中间结点：一个双亲多个孩子 |

# 思维导图
![](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202311180929096.png)
