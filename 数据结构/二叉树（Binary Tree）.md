---
title: 二叉树（BiTree）
date: 2023-11-14 10:55:06
description: 
categories: 
- [数据结构,树]
tags: 
- 二叉树
top_image: 
cover: 
keywords:
- 定义
- 删除
- 创建
- 前序遍历
- 中序遍历
- 后序遍历
---
# 概念
<strong>二叉树（Binary Tree）是n(n>=0)个结点的有限集合，该集合或为<font color = "CC6600">「空集（称为空二叉树）」</font>，或是由一个根结点和两棵<font color = "CC6600">「互不相交的、分别称为根结点的左子树和右子树的二叉树组成」</font>。</strong>因此，我们可以对二叉树进行如下的特点总结：
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
	- 右斜树：二叉树中的所有结点都在右边，这种斜树被称为<font color = "CC6600">「右斜树」</font>
	- 斜树有一个很明显的特征，那就是斜树的