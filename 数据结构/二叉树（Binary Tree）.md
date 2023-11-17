---
title: 二叉树（BiTree）
date: 2023-11-14 10:55:06
description: 这篇文章详细介绍了什么是二叉树、二叉树的五种形态、二叉树的几种特殊情况、二叉树的性质、二叉树的存储结构以及二叉树的三种遍历方法和二叉树的创建
categories:
  - - 数据结构
    - 树
tags:
  - 二叉树
top_image: https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202311172011723.png
cover: https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202311172011723.png
keywords:
  - 定义
  - 删除
  - 创建
  - 前序遍历
  - 中序遍历
  - 后序遍历
  - 推导遍历结果
---
# 概念
<strong>二叉树（Binary Tree）是[[树（Tree）]]的一种特殊情况。在二叉树中，它的每个结点都最多只能有两个孩子</strong>。二叉树有如下三点特征：
- 每个结点<font color = "CC6600">「最多只能」</font>有两棵子树，所以二叉树中不存在度大于2的结点
- 左子树和右子树是<font color = "CC6600">「有顺序的」</font>，次序不能颠倒。
- 即使树中的某一结点只有一棵子树，也要区分它是左子树还是右子树
![953680-20200423142022452-1940672436.png](https://raw.githubusercontent.com/Altholia/CodeNotesPicGo/main/202311141146049.png)

## <font color = "886600">二叉树的五种基本形态</font>
- 空二叉树
- 只有一个根结点
- 根结点只有左子树
- 根结点只有右子树
- 根结点既有左子树由有右子树

# 特殊的二叉树
二叉树具有如下三种特殊性：<font color = "CC6600">「斜树」</font><font color = "CC6600">「满二叉树」</font><font color = "CC6600">「完全二叉树」</font>
- 斜树：斜树又分为<font color = "CC6600">「左斜树」</font>和<font color = "CC6600">「右斜树」</font>
	- 左斜树：二叉树中的所有结点都在左边，这种斜树被称为<font color = "CC6600">「左斜树」</font>
		- ![左斜树.png](https://raw.githubusercontent.com/Altholia/CodeNotesPicGo/main/202311141537520.png)
	- 右斜树：二叉树中的所有结点都在右边，这种斜树被称为<font color = "CC6600">「右斜树」</font>
		- ![右斜树.png](https://raw.githubusercontent.com/Altholia/CodeNotesPicGo/main/202311141539968.png)
	- 斜树有一个很明显的特征，那就是斜树的结点个数就是斜树的<font color = "CC6600">「[[树（Tree）#树的其他概念|深度]]」</font>
- <a id = "满二叉树"><font color = "000000">满二叉树</font></a>：在一棵二叉树中，如果所有的分支结点都有左子树和右子树，并且所有的叶结点都在同一层上，那么这样的二叉树就被称为<font color = "CC6600">「满二叉树」</font>，<font color = "CC6600">「满二叉树」</font>具有如下特点：
	- 叶结点只能出现在最下面一层，出现在其他层就不可能达到平衡
	- 非叶结点的结点的度一定是2
	- 在同样深度的二叉树中，满二叉树的结点个数最多，叶结点最多
	- ![1468919-20191103194220076-925294362.png](https://raw.githubusercontent.com/Altholia/CodeNotesPicGo/main/202311141547851.png)
- 完全二叉树：对一棵具有n个结点的二叉树按层序进行编号，如果编号为i(1<=i<=n)的结点与同样深度的<a href = "#满二叉树"><font color = "CC6600">「满二叉树」</font></a>中编号为i的结点在二叉树中位置完全相同，则这颗二叉树被称为<font color = "CC6600">「完全二叉树」</font>。完全二叉树具有如下特点：
	- 叶结点一定是在最下两层
	- 最下层的叶结点一定集中在左部连续的位置
	- 倒数第二层，若有叶结点，一定都在右部连续位置
	- 如果结点的度为1，则该结点只有左孩子，即不存在只有右孩子的情况
	- 同样结点数的二叉树，完全二叉树的深度最小
	- ![1345862-20200701231304231-2079073084.png](https://raw.githubusercontent.com/Altholia/CodeNotesPicGo/main/202311141601929.png)

# 二叉树的性质
<strong>be continue……</strong>