---
title: 二叉树（BiTree）
date: 2023-11-14 10:55:06
description: 这篇文章详细介绍了什么是二叉树、二叉树的五种形态、二叉树的几种特殊情况、二叉树的性质、二叉树的存储结构以及二叉树的三种遍历方法和二叉树的创建
categories:
  - - 数据结构
    - 树
tags:
  - 二叉树
  - C#
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
  - 存储结构
  - 二叉树的性质
  - 二叉树的抽象数据类型
---
# 概念
二叉树（Binary Tree）是[[树（Tree）]]的一种。在二叉树中，它的每个结点都<font color = "CC6600">「最多只能有两个孩子」</font>。二叉树有如下三点特征：
- 每个结点<font color = "CC6600">「最多只能」</font>有两棵子树，所以二叉树中不存在度大于2的结点
- 左子树和右子树是<font color = "CC6600">「有顺序的」</font>，次序不能颠倒。
- 即使树中的某一结点只有一棵子树，也要区分它是左子树还是右子树
![](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202311180926064.png)

## <font color = "886600">二叉树的五种基本形态</font>
- 空二叉树
- 只有一个根结点
- 根结点只有左子树
- 根结点只有右子树
- 根结点既有左子树又有右子树

# 特殊的二叉树
二叉树具有如下三种特殊性：<font color = "CC6600">「斜树」</font><font color = "CC6600">「满二叉树」</font><font color = "CC6600">「完全二叉树」</font>
- 斜树：斜树又分为<font color = "CC6600">「左斜树」</font>和<font color = "CC6600">「右斜树」</font>
	- 左斜树：二叉树中的所有结点都在左边，这种斜树被称为<font color = "CC6600">「左斜树」</font>
		- ![](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202311180927205.png)
	- 右斜树：二叉树中的所有结点都在右边，这种斜树被称为<font color = "CC6600">「右斜树」</font>
		- ![](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202311180927048.png)
	- 斜树有一个很明显的特征，那就是斜树的结点个数就是斜树的<font color = "CC6600">「[[树（Tree）#树的其他概念|深度]]」</font>
- <a id = "满二叉树"><font color = "000000">满二叉树</font></a>：在一棵二叉树中，如果所有的分支结点都有左子树和右子树，并且所有的叶结点都在同一层上，那么这样的二叉树就被称为<font color = "CC6600">「满二叉树」</font>，<font color = "CC6600">「满二叉树」</font>具有如下特点： ^99da65
	- 叶结点只能出现在最下面一层，出现在其他层就不可能达到平衡
	- 非叶结点的结点的度一定是2
	- 在同样深度的二叉树中，满二叉树的结点个数最多，叶结点最多
	- ![](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202311180928704.png))
- 完全二叉树：对一棵具有n个结点的二叉树按层序进行编号，如果编号为i(1<=i<=n)的结点与同样深度的<a href = "#满二叉树"><font color = "CC6600">「满二叉树」</font></a>中编号为i的结点在二叉树中位置完全相同，则这颗二叉树被称为<font color = "CC6600">「完全二叉树」</font>。完全二叉树具有如下特点：
	- 叶结点一定是在最下两层
	- 最下层的叶结点一定集中在左部连续的位置
	- 倒数第二层，若有叶结点，一定都在右部连续位置
	- 如果结点的度为1，则该结点只有左孩子，即不存在只有右孩子的情况
	- 同样结点数的二叉树，完全二叉树的深度最小
	- ![](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202311180928889.png)

# 二叉树的性质

^439a99

<strong>对于二叉树，它具有五种基础的性质：</strong>
- 性质1：在二叉树的第i层<font color = "CC6600">「至多」</font>有<font color = "CC6600">「2<sup>i-1</sup>」</font>个结点（i>=1）
	- 例如，在<font color = "CC6600">「[[二叉树（Binary Tree）#^99da65|满二叉树]]」</font>中，他的第三层，则最多有：2<sup>i-1</sup>=2<sup>3-1</sup>=4个结点![](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202311180928704.png)
- 性质2：深度为K的二叉树至多有<font color = "CC6600">「2<sup>k</sup>-1」</font>个结点
	- 如上图所示，如果树的是深度是1，则这棵树的<font color = "CC6600">「总结点数为」</font>：2<sup>k</sup>-1=2<sup>1</sup>-1=1个结点；如果深度是2则：2<sup>k</sup>-1=2<sup>2</sup>-1=3
	- <font color = "BA8448">【需要注意的是，该公式计算的是整棵树的至多结点个数，而不是单独每一层】</font>
- 性质3：对任何一棵二叉树T，如果其叶结点（终端结点）数为n<sub>0</sub>，度为2的结点数为n<sub>2</sub>，则n<sub>0</sub>=n<sub>2</sub>+1
	- 例如，在下图中，度为2的结点个数为4，而叶结点的个数为n<sub>0</sub>=n<sub>2</sub>+1=5![](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202311180928889.png)
- 性质4：具有n个结点的<font color = "CC6600">「完全二叉树」</font>的深度为\[log<sub>2</sub>n+1](\[x](这里是☞log<sub>2</sub>n)表示不大于x的最大整数)
- 性质5：如果对一棵有n个结点的<font color = "CC6600">「完全二叉树」</font>（其深度为\[log<sub>2</sub>n+1]）的结点按层序编号（从第1层到第\[log<sub>2</sub>n+1]层，每层从左到右），对任一结点i(1<=i<=n)有：
	- 如果i=1，则结点i是二叉树的根，无双亲；如果i>1，则其双亲是结点\[i/2(注意这里是<font color = "CC6600">「向下取整」</font>)]
	- 如果2i>n，则结点i无左孩子（结点i为叶子结点）；否则其左孩子是结点<font color = "CC6600">「2i」</font>；
	- 如果2i+1>n，则结点i无右孩子；否则其右孩子是结点<font color = "CC6600">「2i+1」</font>
	- 如图所示![](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202311180928889.png)：
		- 针对第一点来说，当i=1(A)时，毫无疑问该结点是根结点，而当i>1时，例如i=3(C)，则它的双亲就是3/2=1(A)（向下取整），或者当i=7(G)时，它的双亲是7/2=3(C)
		- 针对第二点，当i=6(F)时，2i=12>10，则F结点是叶结点。而当2i=10(J)时，该结点具有左孩子J
		- 针对第三点，众所周知，在二叉树中左右结点是相邻的，即右结点是：2i+1。如果2i+1>n则该结点无右孩子，例如当i=5(E)时，2i+1=11>10，则该结点没有右孩子，而当i=4(E)时，2i+1=9(I)，所以该结点的右孩子是I

# 二叉树的存储结构
## <font color = "886600">二叉树的顺序存储结构</font>
<strong>二叉树的顺序存储结构是利用一维数组进行存储的</strong>
- 对于顺序存储结构来说，结点存储的位置，也就是数组的小标需要体现出结点之间的逻辑关系（也就是可用利用性质5来计算双亲、左结点和右结点的下标）
- 得益于<font color = "CC6600">「完全二叉树」</font>的严格定义，我们可以很容易的使用数组来存储完全二叉树

如图所示，这是一个完全二叉树：
![](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202311180928889.png)
它在数组中的存储如下表所示（其实要得出这个一维数组其实不难，对完全二叉树进行[[二叉树（Binary Tree）#^2ed6f7 | 层序遍历]]就可以了）：

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:--:|
| A | B | C | D | E | F | G | H | I | J |
- 对于B(2)结点，它的左孩子是：2i=4，右孩子是：2i+1=5，双亲是：i/2=1
- 对于E(5)结点，它的左孩子是：2i=10，双亲是：i/2=2
- 对于G(7)结点，它2i>n，2i+1>n，所以无左右孩子，双亲是：i/2=3

从上述计算过程中可以看出，对于完全二叉树我们可以非常容易地使用数组来实现，但如果是普通的二叉树就那么好实现了，因为它的结点可能是不连续得（结点与结点之间存在Null），如下图所示（虚线表示为空）：
![image.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202311181741105.png)
那么对于这种情况，我们需要将它“变成”完全二叉树，也就是在通过一维数组对二叉树进行存储的时候将Null值也存储进去，如下表所示：

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:--:|
| A | B | C | ^ | E | ^ | G | ^ | ^ | J |

但是这样有一个缺点，那就是会浪费存储空间。例如我们考虑一种极端情况，如果这棵二叉树是右斜树，它只有K个结点，它却需要分配2<sup>k</sup>-1个存储单元空间，这就极大的浪费了存储空间，<font color = "BA8448">【所以我们一般只利用顺序存储结构来存储完全二叉树】</font>
![image.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202311181749499.png)

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 |
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:--:|:--:|:--:|:--:|:--:|:--:|
| A | ^ | B | ^ | ^ | ^ | C | ^ | ^ | J | ^ | ^ | ^ | ^ | D |

## <font color = "886600">二叉树的链式存储结构</font>
<strong>对于二叉树，它的每个结点最多只有两个结点，那么我们就给它设计一个数据域和两个指针域，这样的链表被称为<font color = "CC6600">「二叉链表」</font></strong>

| leftChild | rightChild | data |
|:---------:|:----------:|:----:|

二叉链表的结点结构定义代码如下：
```C#
public class BiTree<T>
{
	public T Data {get;set;} //存储数据信息
	public BiTree? LChild {get;set;} //存储左孩子
	public BiTree? RChild {get;set;} //存储右孩子
	public BiTree? PChild {get;set;} //根据需要可以添加也可以不添加，用来
	//存储父结点，添加了父结点的链表被称为三叉链表（以此类推）
}
```
# 二叉树的遍历
<strong>对于二叉树的遍历是指从根结点出发，按照某种<font color = "CC6600">「次序」</font>依次<font color = "CC6600">「访问」</font>二叉树中的所有结点，使得每个结点被访问一次且仅被访问一次</strong>
- 访问：访问其实就是根据实际的需要要来确定具体做什么，比如对每个结点进行相关计算，输出打印等，它算作是一个抽象的操作
- 次序：二叉树的遍历不同于线性结构，线性结构最多就是从头至尾、循环、双向等简单的遍历方式。而二叉树的结点之间不存在前序和后继关系，在访问一个结点之后，下一个被访问的结点面临着不同的选择，因此对于二叉树的遍历前辈们总结出了四种遍历方式：<font color = "CC6600">「层序遍历」</font><font color = "CC6600">「前序遍历」</font><font color = "CC6600">「中序遍历」</font><font color = "CC6600">「后序遍历」</font>
## <font color = "886600">层序遍历</font>
<strong>如果二叉树是空，则返回Null，否则从树的第一层，也就就是根结点开始访问，从上而下逐层遍历，在同一层中，按从左到右的顺序对结点逐个访问</strong>， ^2ed6f7
层序遍历的流程图如下所示：
![image.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202311191753185.png)
代码如下：
```C#
public IEnumerable<BiTreeNode<T>> GetLevelOrderTraverse()
{
    if (Count == 0)
        throw new ArgumentNullException(nameof(_headNode));

    BiTreeNode<T>? node = _headNode;

    List<BiTreeNode<T>> result = new();
    Queue<BiTreeNode<T>> queue = new();

    result.Add(node);
    queue.Enqueue(node.LeftChild);
    queue.Enqueue(node.RightChild);

    while (queue.Count != 0)
    {
        node = queue.Dequeue();

        if (node is not null)
        {
            result.Add(node);
            queue.Enqueue(node.LeftChild);
            queue.Enqueue(node.RightChild);
        }
    }

    return result;
}
```
- 在层序遍历的代码中，首先获得根结点，并将它存储在结果集中
- 之后利用[[队列(Queue)]]存储当前结点的左右结点。由于第一个结点是根结点，所以利用<font color = "CC6600">「队列」</font>可以将第二层的结点全部入队
- 进入循环：
	- 由于队列遵循FIFO原则，因此在第一次循环的过程中出队的是根结点的左孩子，即第二层从左往右的第一个结点。
	- 如果出队的结点不为空则进入if语句中将该结点添加至结果集中，然后再将该结点的左右结点进行入队操作
	- 如此循环，直至遍历完
- 返回结果集
## <font color = "886600">前序遍历</font>
<strong>如果二叉树为空则返回Null，否则先访问根结点，然后前序遍历左子树，再前序遍历右子树</strong>
- 小白解释：因为这是前序遍历，所以我们需要先访问根结点（可以是打印根结点，也可以对根结点做其他的操作，看个人需求），然后再访问根结点的左结点，如果左结点还有左结点就接着访问左结点，直到访问到最后一个左结点为止。如果最后一个左结点既没有左结点也没有右结点就返回到上一个结点，然后访问该结点的右结点，如果右结点有左结点就依然是先访问该右结点的左结点，然后再访问右结点，依次循环。请看图：
- ![image.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202311191818313.png)
代码如下所示：
```C#
public IEnumerable<T> GetPreOrderTraverse()
{
    if(Count == 0)
        throw new InvalidOperationException("The binary tree is empty");

    List<T> result = new();
    BiTreeNode<T> root = _headNode;
    PreOrderTraverse(root);

    void PreOrderTraverse(BiTreeNode<T>? rootNode)
    {
        if (rootNode is null)
            return;

        result.Add(rootNode.Data);//核心代码
        PreOrderTraverse(rootNode.LeftChild);//核心代码
        PreOrderTraverse(rootNode.RightChild);//核心代码
    }

    return result;
}
```
- 前序遍历通过递归来完成。首先我们将根结点传递给PreOrderTraverse方法
	- 判断结点是否为空，如果为空则返回。否则进行下一步
	- 将该结点的数据添加至结果集中
	- 然后递归左结点
	- 最后递归右结点
- 返回结果集
## <font color = "886600">中序遍历</font>
<strong>若树为空则返回NULL，否则从根结点开始（注意是开始而不是访问根结点），中序遍历根结点的左子树，然后是访问根结点，最后中序遍历右子树</strong>
- 小白解释：中序遍历将树以根结点为基准划分成了左右两部分。
中序遍历的流程如图所示：
![image(1).png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202311192004913.png)
代码如下：
```C#
public IEnumerable<T> GetInOrderTraverse()
{
    if(Count == 0)
        throw new InvalidOperationException("The binary tree is empty");

    List<T> result = new();
    BiTreeNode<T> root = _headNode;

    InOrderTraverse(root);

    void InOrderTraverse(BiTreeNode<T>? rootNode)
    {
        if(rootNode is null)
            return;

        InOrderTraverse(rootNode.LeftChild);//核心代码
        result.Add(rootNode.Data);//核心代码
        InOrderTraverse(rootNode.RightChild);//核心代码
    }

    return result;
}
```
- 中序遍历的完成也是通过递归才实现的，与<font color = "CC6600">「前序遍历」</font>基本一致，唯一的不同就是核心代码的执行顺序有所改变
## <font color = "886600">后序遍历</font>
<strong>如果树为空则返回NULL，否则从左到右先叶结点后双亲结点的方式遍历访问左右子树，最后是访问根结点</strong>
后序遍历的流程图如下所示：
![image.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202311192009607.png)
代码如下所示：
```C#
public IEnumerable<T> GetPostOrderTraverse()
{
    if(Count == 0)
        throw new InvalidOperationException("The binary tree is empty");

    List<T> result = new();
    BiTreeNode<T> root = _headNode;

    PostOrderTraverse(root);

    void PostOrderTraverse(BiTreeNode<T>? rootNode)
    {
        if (rootNode is null)
            return;

        PostOrderTraverse(rootNode.LeftChild);//核心代码
        PostOrderTraverse(rootNode.RightChild);//核心代码
        result.Add(rootNode.Data);//核心代码
    }

    return result;
}
```
## <font color = "886600">推导遍历结果</font>
- 定义如下：
	- <strong>已知二叉树的前序遍历和中序遍历可以唯一确定一棵二叉树</strong>
	- <strong>已知二叉树的后序遍历和中序遍历可以唯一确定一棵二叉树</strong>
	- <strong>已知后序遍历和前序遍历无法唯一确定一棵二叉树</strong>
		- 因为我们无法确认该结点是右结点还是左结点
- 推导方法：
	1. 首先根据前（后）序遍历结果确定根结点
		- 因为在前序遍历中，根结点总是在最前面；同样的，后序遍历的根结点总是在最后面
	2. 我们以根结点为中心点，将中序遍历的结果分为左子树和右子树
	3. 在前序遍历中寻找到相应的左子树和右子树
	4. 在前（后）序遍历中，左（右）子树开头（结尾）的那个结点就是相应子树的根结点
	5. 重复第2步
- 例：根据前序遍历和中序遍历推导出二叉树
	- ![image.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202311220912909.png)
- 例：根据后序遍历和中序遍历推导出一棵二叉树
	- ![image.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202311220940578.png)

# 二叉树的数据结构
```C#
public class Node<T>
{
	#pragma warning disable CS8618
    public T Data { get; set; }
#pragma warning restore CS8618

    public Node<T>? LeftNode { get; set; }
    public Node<T>? RightNode { get; set; }
    public Node<T>? ParentNode { get; set; }
}
```

# 二叉树的抽象数据类型
```C#
ADT 二叉树(Binary Tree)
Data
	二叉树仅由一个根结点和若干棵子树构成，它的数据对象集合为{a1,a2,a3,a4,……,an}，每个数据元素的类型均为DataType且具有一定的层次关系
Operation
	void Constructor()：构造方法，创建一棵空树
	void Constructor(IEnumerable<T> preOrder,IEnumerable<T> inOrder)：根据前序遍历和中序遍历构造一棵二叉树
	T GetNodeVal(int index)：获取指定索引处的结点的值
	int GetLeafCount()：获取二叉树中叶结点的个数
	int GetDepth()：获取二叉树的深度
	void AddNode(T val,int index,LefOrRigEnum rightOrRight)：添加新结点，其中，LefOrRigEnum是一个枚举，里面包含Left和Right
	void RemoveNode(int index)：删除指定处的结点
	bool Contains(T val)：查找在二叉树中是否存在val这个值
	IEnumerable<T> LevelOrderTraversal()：获取二叉树的层序遍历
	IEnumerable<T> PreOrderTraversal()：获取二叉树的前序遍历
	IEnumerable<T> OrderTraversal()：获取二叉树的中序遍历
	IEnumerable<T> PostOrderTraversal()：获取二叉树的后序遍历
	bool IsEmpty()：判断二叉树是否为空
	void Clear()：清空二叉树
	void Dispose()：释放二叉树
endADT
```
# 二叉树的创建
- 我们已知：
	- 根据<font color = "CC6600">「前序遍历」</font>和<font color = "CC6600">「中序遍历」</font>可以唯一确定一棵二叉树
	- 根据<font color = "CC6600">「后序遍历」</font>和<font color = "CC6600">「中序遍历」</font>可以唯一确定一棵二叉树
- 因此我们可以根据上述两条性质来创建二叉树
	- 根据<font color = "CC6600">「前序遍历&&中序遍历」</font>来创建一棵二叉树
	- 根据<font color = "CC6600">「后序遍历&&中序遍历」</font>来创建一棵二叉树

## <font color = "886600">前序遍历&&中序遍历</font>
```C#
    /// <summary>
    /// 根据前序遍历结果和中序遍历结果构造二叉树
    /// </summary>
    /// <param name="preOrder">前序遍历结果</param>
    /// <param name="inOrder">后序遍历结果</param>
    /// <param name="parentNode">父节点，默认为null</param>
    /// <returns>返回根节点</returns>
    private static BiTreeNode<T>? BuildTreeByPreOrder(T[] preOrder, T[] inOrder,BiTreeNode<T>? parentNode = null)
    {
        if (preOrder.Length == 0)
            return null;

        T rootData = preOrder[0];
        int rootIndex = Array.IndexOf(inOrder, rootData);

        T[] leftSubTreeInOrder = inOrder[..rootIndex];//获取中序遍历的左子树
        T[] rightSubTreeInOrder = inOrder[(rootIndex + 1)..];//获取中序遍历的右子树

        T[] leftSubTreePreOrder = preOrder[1..(leftSubTreeInOrder.Length+1)];//获取前序遍历的左子树
        T[] rightSubTreePreOrder = preOrder[(leftSubTreeInOrder.Length+1)..];//获取前序遍历的右子树

        BiTreeNode<T> rootNode = new()
        {
            Data = rootData,
            ParentNode = parentNode
        };

        rootNode.LeftChild = BuildTreeByPreOrder(leftSubTreePreOrder, leftSubTreeInOrder,rootNode);
        rootNode.RightChild = 
            BuildTreeByPreOrder(rightSubTreePreOrder, rightSubTreeInOrder,rootNode);

        return rootNode;
    }
```

## <font color = "886600">后序遍历&&中序遍历</font>
```C#
/// <summary>
/// 根据后序遍历结果和中序遍历结果构造二叉树
/// </summary>
/// <param name="postOrder">后序遍历结果</param>
/// <param name="inOrder">中序遍历结果</param>
/// <param name="parentNode">父节点，默认为null</param>
/// <returns>返回根节点</returns>
private static BiTreeNode<T>? BuildTreeByPostOrder(T[] postOrder, T[] inOrder,BiTreeNode<T>? parentNode = null)
{
    if (postOrder.Length == 0)
        return null;

    T rootData = postOrder[^1];
    int rootIndex = Array.IndexOf(inOrder, rootData);
    if(rootIndex < 0)
        throw new InvalidOperationException("The data in the postOrder traversal result does not exist in the inOrder traversal result");

    T[] leftSubTreeInOrder = inOrder[..rootIndex];//获取中序遍历的左子树
    T[] rightSubTreeInOrder = inOrder[(rootIndex + 1)..];//获取中序遍历的右子树

    T[] leftSubTreePostOrder = postOrder[..leftSubTreeInOrder.Length];//获取后序遍历的左子树
    T[] rightSubTreePostOrder = postOrder[leftSubTreeInOrder.Length..^1];//获取后序遍历的右子树

    BiTreeNode<T> rootNode = new()
    {
        Data = rootData,
        ParentNode = parentNode
    };

    rootNode.LeftChild = BuildTreeByPostOrder(leftSubTreePostOrder, leftSubTreeInOrder,rootNode);
    rootNode.RightChild = BuildTreeByPostOrder(rightSubTreePostOrder, rightSubTreeInOrder, rootNode);

    return rootNode;
}
```

# 二叉树的删除
二叉树的删除分为三种情况：<font color = "CC6600">「删除的结点是叶结点」</font><font color = "CC6600">「删除的结点只有左结点（右结点）」</font><font color = "CC6600">「删除的结点即有左结点又有右结点」</font>
## <font color = "886600">删除叶结点</font>
这种情况是最简单的，直接令叶结点为null即可：
```
//如果删除的结点是叶结点
if (deleteNode.LeftNode is null && deleteNode.RightNode is null)
{
    //先判断被删除的结点是父结点的左子树还是右子树
    if(deleteNode.ParentNode!.LeftNode==deleteNode)
        deleteNode.ParentNode.LeftNode = null;//如果是左子树，则将左子树置空
    else
        deleteNode.ParentNode.RightNode = null;//如果是右子树，则将右子树置空

    Count--;
    return;
}
```

## <font color = "886600">删除的结点具有左（右）子结点</font>
如果删除的结点具有左（右）子结点，则我们需要将删除的结点的父结点指向这个左（右）子结点，具体请看代码：
```C#
//如果删除的结点只有左子树
if (deleteNode.LeftNode is not null && deleteNode.RightNode is null)
{
    //判断删除的结点是父结点的左子树还是右子树
    if (deleteNode.ParentNode!.LeftNode == deleteNode)
    {
        //如果是左子树，则将删除结点的左子树作为父结点的左子树，并将删除结点的左子树的父结点指向父结点
        deleteNode.ParentNode.LeftNode = deleteNode.LeftNode;
        deleteNode.LeftNode.ParentNode = deleteNode.ParentNode;
    }
    else
    {
        //如果是右子树，则将删除结点的左子树作为父结点的右子树，并将删除结点的左子树的父结点指向父结点
        deleteNode.ParentNode.RightNode = deleteNode.LeftNode;
        deleteNode.LeftNode.ParentNode = deleteNode.ParentNode;
    }

    Count--;
    return;
}

//如果删除的结点只有右子树
if (deleteNode.RightNode is not null && deleteNode.LeftNode is null)
{
    //如果删除的结点是父结点的左子树
    if (deleteNode.ParentNode!.LeftNode == deleteNode)
    {
        deleteNode.ParentNode.LeftNode = deleteNode.RightNode;
        deleteNode.RightNode.ParentNode = deleteNode.ParentNode;
    }
    else
    {
        //如果删除的结点是父结点的右子树
        deleteNode.ParentNode.RightNode = deleteNode.RightNode;
        deleteNode.RightNode.ParentNode = deleteNode.ParentNode;
    }

    Count--;
    return;
}
```

## <font color = "886600">删除的结点同时具有左右结点</font>
这种情况最为复杂，因此具有两种删除方法：
- 第一种方法：找到删除结点的左子树的最右（大）的那颗结点，然后替换掉被删除的那颗结点，看图（这里我们假设删除B结点）：
	- ![image.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202311261827767.png)
- 第二种方法：找到删除结点的右子树中最左（小）的那颗结点，然后替换掉被删除的那颗结点，看图（这里我们假设删除C结点）：
	- ![image.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202311261832573.png)

代码如下：
```C#
//如果删除的结点既有左子树又有右子树
if (deleteNode.LeftNode is not null && deleteNode.RightNode is not null)
{
    //如果删除的结点的左子树没有右子树
    if (deleteNode.LeftNode.RightNode is null)
    {
        deleteNode.Data = deleteNode.LeftNode.Data;
        deleteNode.LeftNode = deleteNode.LeftNode.LeftNode;
        if(deleteNode.LeftNode is not null)
            deleteNode.LeftNode.ParentNode = deleteNode;

        Count--;
        return;
    }

    //如果删除的结点的左子树有右子树
    if (deleteNode.LeftNode!.RightNode is not null)
    {
        Node<T> rightNode = deleteNode.LeftNode.RightNode;
        while (rightNode.RightNode is not null)
            rightNode = rightNode.RightNode;

        deleteNode.Data = rightNode.Data;

        Node<T> rightNodeParent = rightNode.ParentNode!;
        rightNode = rightNode.LeftNode!;
        if (rightNode is not null)
        {
            rightNodeParent.RightNode = rightNode;
            rightNode.ParentNode = rightNodeParent;
        }
        else
            rightNodeParent.RightNode = null;

        Count--;
    }
}
```

# 示例代码：
```
public class Node<T>
{
#pragma warning disable CS8618
    public T Data { get; set; }
#pragma warning restore CS8618
    public Node<T>? LeftNode { get; set; }
    public Node<T>? RightNode { get; set; }
    public Node<T>? ParentNode { get; set; }
}

public interface IBiTree<T> : IDisposable
{
    T GetNodeVal(int index);//获取指定索引处的结点值
    int GetLeafCount();//获取叶子结点个数
    int GetDepth();//获取树的深度
    void AddNode(T val,int index,LefOrRigEnum lefOrRig);//添加结点
    void RemoveNode(int index);//删除结点
    bool Contains(T val);//判断是否包含指定值的结点
    bool IsEmpty();
    IEnumerable<T> LevelOrderTraversal();//层序遍历
    IEnumerable<T> PreOrderTraversal();//前序遍历
    IEnumerable<T> OrderTraversal();//中序遍历
    IEnumerable<T> PostOrderTraversal();//后序遍历
    void Clear();//清空树
}

public class BiTree<T> : IBiTree<T>
{
    private Node<T> _headNode;

    public BiTree()
    {
        Count = 0;
        _headNode = null!;
    }

    public int Count { get; private set; }

    /// <summary>
    /// 构造函数，根据前序遍历和中序遍历结果构造一棵二叉树
    /// </summary>
    /// <param name="preOrder">前序遍历结果</param=
    /// <param name="inOrder">中序遍历结果</param>
    /// <exception cref="ArgumentNullException">如果前序遍历或中序遍历结果为空，则抛出异常</exception>
    /// <exception cref="InvalidOperationException">如果前序遍历和中序遍历的数据元素个数不一致则抛出异常</exception>
    public BiTree(IEnumerable<T> preOrder, IEnumerable<T> inOrder)
    {
        if(preOrder is null)
            throw new ArgumentNullException(nameof(preOrder),"preOrder is NULL");

        if (inOrder is null)
            throw new ArgumentNullException(nameof(inOrder), "inOrder is NULL");

        List<T> preList = preOrder.ToList();
        List<T> inList = inOrder.ToList();

        if (preList.Count==0)
            throw new InvalidOperationException("preOrder doesn't have any data");

        if (inList.Count==0)
            throw new InvalidOperationException("inOrder doesn't have any data");

        if (preList.Count != inList.Count)
            throw new InvalidOperationException("The array length is inconsistent");

        Count = preList.Count;
        _headNode = BuilderBiTree(preList, inList);
    }

    /// <summary>
    /// 根据前序遍历结果和中序遍历结果构造一棵二叉树
    /// </summary>
    /// <param name="preOrder">前序遍历</param>
    /// <param name="inOrder">中序遍历</param>
    /// <param name="parNode">结点</param>
    /// <returns>返回根结点</returns>
    private static Node<T> BuilderBiTree(List<T>? preOrder, List<T>? inOrder, Node<T>? parNode = null)
    {
        if (preOrder is null || inOrder is null || preOrder.Count == 0 || inOrder.Count == 0)
            return null!;

        T rootVal = preOrder[0];//获取根节点的值
        int rootIndex = inOrder.IndexOf(rootVal);//获取根节点在中序遍历中的位置

        List<T> leftInOrder = inOrder[..rootIndex];//获取中序遍历的左子树
        List<T> rightInOrder = inOrder[(rootIndex + 1)..];//获取中序遍历的右子树

        List<T> leftPreOrder = preOrder[1..(leftInOrder.Count + 1)];
        List<T> rightPreOrder = preOrder[(leftInOrder.Count + 1)..];

        Node<T> node = new()
        {
            Data = rootVal,
            ParentNode = parNode
        };

        node.LeftNode = BuilderBiTree(leftPreOrder, leftInOrder, node);
        node.RightNode = BuilderBiTree(rightPreOrder, rightInOrder, node);

        return node;
    }

    /// <summary>
    /// 根据层序遍历的结果获取指定索引的结点的值
    /// </summary>
    /// <param name="index">指定索引</param>
    /// <returns>返回结点的值</returns>
    /// <exception cref="IndexOutOfRangeException">如果指定的索引超出范围则抛出异常</exception>
    public T GetNodeVal(int index)
    {
        if(index < 0 || index >= Count)
            throw new IndexOutOfRangeException(nameof(index));

        return LevelOrderTraversal().ElementAt(index);
    }

#if DEBUG
    public Node<T> GetNode(int index)
    {
        if (index < 0 || index >= Count)
            throw new IndexOutOfRangeException(nameof(index));

        return LevelOrderTraversalNodes()?[index]
               ?? throw new InvalidOperationException("internal error,please contact admin");
    }
#endif

    /// <summary>
    /// 统计叶结点的个数
    /// </summary>
    /// <returns></returns>
    /// <exception cref="ArgumentNullException">如果二叉树为空则抛出异常</exception>
    /// <exception cref="InvalidOperationException">如果通过层序遍历返回的结果是NULL则抛出异常</exception>
    public int GetLeafCount()
    {
        if (IsEmpty())
            throw new ArgumentNullException(nameof(_headNode), "The tree is empty");

        IReadOnlyList<Node<T>> nodes = LevelOrderTraversalNodes() ?? throw new InvalidOperationException("internal error,please contact admin");

        return nodes.Count(node => node.LeftNode is null && node.RightNode is null);
    }

    /// <summary>
    /// 计算二叉树的深度
    /// </summary>
    /// <returns>返回二叉树的深度</returns>
    public int GetDepth() => GetDepth(_headNode);

    /// <summary>
    /// 私有方法，用于计算二叉树的深度
    /// </summary>
    /// <param name="rootNode">根结点</param>
    /// <returns>返回树的最大深度</returns>
    private int GetDepth(Node<T>? rootNode)
    {
        if (rootNode is null || Count == 0)
            return 0;

        int leftDepth = GetDepth(rootNode.LeftNode);
        int rightDepth = GetDepth(rootNode.RightNode);

        return Math.Max(leftDepth, rightDepth) + 1;
    }

    /// <summary>
    /// 为指定索引处的结点添加子结点
    /// </summary>
    /// <param name="val">子结点的值</param>
    /// <param name="index">父结点的索引</param>
    /// <param name="rightOrRight">是添加为左结点还是添加为右结点</param>
    /// <exception cref="ArgumentNullException">如果添加的值为空则抛出异常</exception>
    /// <exception cref="IndexOutOfRangeException">如果索引超出范围则抛出异常</exception>
    /// <exception cref="InvalidOperationException">如果返回的父结点为空则抛出异常</exception>
    /// <exception cref="ArgumentOutOfRangeException"></exception>
    public void AddNode(T val,int index,LefOrRigEnum rightOrRight)
    {
        if (val is null)
            throw new ArgumentNullException(nameof(val), "val is NULL");
        if (index < 0 || index >= Count)
            throw new IndexOutOfRangeException(nameof(index));

        Node<T> parentNode = LevelOrderTraversalNodes()?[index] ?? throw new InvalidOperationException("internal error,please contact admin");

        switch (rightOrRight)
        {
            case LefOrRigEnum.Left:
                if (parentNode.LeftNode is not null)
                    throw new InvalidOperationException("tree have left node");
                parentNode.LeftNode = new Node<T>
                {
                    Data = val,
                    ParentNode = parentNode
                };
                Count++;
                break;
            case LefOrRigEnum.Right:
                if (parentNode.RightNode is not null)
                    throw new InvalidOperationException("tree have right node");
                parentNode.RightNode = new Node<T>
                {
                    Data = val,
                    ParentNode = parentNode
                };
                Count++;
                break;
            default:
                throw new ArgumentOutOfRangeException(nameof(rightOrRight));
        }
    }

    /// <summary>
    /// 删除二叉树中的指定索引处的结点（注意，这里没有添加删除根结点的有关操作）
    /// </summary>
    /// <param name="index">需要删除的结点</param>
    /// <exception cref="IndexOutOfRangeException">如果索引超出范围则抛出异常</exception>
    /// <exception cref="InvalidOperationException">如果没有查找到需要删除的结点则抛出异常</exception>
    public void RemoveNode(int index)
    {
        if (index < 0 || index >= Count)
            throw new IndexOutOfRangeException(nameof(index));

        Node<T> deleteNode = LevelOrderTraversalNodes()?[index]
                       ?? throw new InvalidOperationException("internal error,please contact admin");

        //如果删除的结点是叶结点
        if (deleteNode.LeftNode is null && deleteNode.RightNode is null)
        {
            //先判断被删除的结点是父结点的左子树还是右子树
            if(deleteNode.ParentNode!.LeftNode==deleteNode)
                deleteNode.ParentNode.LeftNode = null;//如果是左子树，则将左子树置空
            else
                deleteNode.ParentNode.RightNode = null;//如果是右子树，则将右子树置空

            Count--;
            return;
        }

        //如果删除的结点只有左子树
        if (deleteNode.LeftNode is not null && deleteNode.RightNode is null)
        {
            //判断删除的结点是父结点的左子树还是右子树
            if (deleteNode.ParentNode!.LeftNode == deleteNode)
            {
                //如果是左子树，则将删除结点的左子树作为父结点的左子树，并将删除结点的左子树的父结点指向父结点
                deleteNode.ParentNode.LeftNode = deleteNode.LeftNode;
                deleteNode.LeftNode.ParentNode = deleteNode.ParentNode;
            }
            else
            {
                //如果是右子树，则将删除结点的左子树作为父结点的右子树，并将删除结点的左子树的父结点指向父结点
                deleteNode.ParentNode.RightNode = deleteNode.LeftNode;
                deleteNode.LeftNode.ParentNode = deleteNode.ParentNode;
            }

            Count--;
            return;
        }

        //如果删除的结点只有右子树
        if (deleteNode.RightNode is not null && deleteNode.LeftNode is null)
        {
            //如果删除的结点是父结点的左子树
            if (deleteNode.ParentNode!.LeftNode == deleteNode)
            {
                deleteNode.ParentNode.LeftNode = deleteNode.RightNode;
                deleteNode.RightNode.ParentNode = deleteNode.ParentNode;
            }
            else
            {
                //如果删除的结点是父结点的右子树
                deleteNode.ParentNode.RightNode = deleteNode.RightNode;
                deleteNode.RightNode.ParentNode = deleteNode.ParentNode;
            }

            Count--;
            return;
        }

        //如果删除的结点既有左子树又有右子树
        if (deleteNode.LeftNode is not null && deleteNode.RightNode is not null)
        {
            //如果删除的结点的左子树没有右子树
            if (deleteNode.LeftNode.RightNode is null)
            {
                deleteNode.Data = deleteNode.LeftNode.Data;
                deleteNode.LeftNode = deleteNode.LeftNode.LeftNode;
                if(deleteNode.LeftNode is not null)
                    deleteNode.LeftNode.ParentNode = deleteNode;

                Count--;
                return;
            }

            //如果删除的结点的左子树有右子树
            if (deleteNode.LeftNode!.RightNode is not null)
            {
                Node<T> rightNode = deleteNode.LeftNode.RightNode;
                while (rightNode.RightNode is not null)
                    rightNode = rightNode.RightNode;

                deleteNode.Data = rightNode.Data;

                Node<T> rightNodeParent = rightNode.ParentNode!;
                rightNode = rightNode.LeftNode!;
                if (rightNode is not null)
                {
                    rightNodeParent.RightNode = rightNode;
                    rightNode.ParentNode = rightNodeParent;
                }
                else
                    rightNodeParent.RightNode = null;

                Count--;
            }
        }
    }

    /// <summary>
    /// 查看二叉树中是否有指定的值
    /// </summary>
    /// <param name="val">需要确认是否存在的值</param>
    /// <returns>如果有，则返回true;否则返回false</returns>
    public bool Contains(T val) => LevelOrderTraversal().Contains(val);

    /// <summary>
    /// 层序遍历二叉树
    /// </summary>
    /// <returns>返回结点值的集合</returns>
    /// <exception cref="InvalidOperationException">如果二叉树为空则抛出异常</exception>
    public IEnumerable<T> LevelOrderTraversal()
    {
        if(IsEmpty())
            throw new InvalidOperationException("The tree is empty");

        return LevelOrderTraversalNodes()?.Select(node => node.Data)
               ?? throw new InvalidOperationException("internal error,please contact admin");
    }

    /// <summary>
    /// 内部方法，用于层序遍历二叉树
    /// </summary>
    /// <returns>返回结点集</returns>
    private IReadOnlyList<Node<T>>? LevelOrderTraversalNodes()
    {
        if (IsEmpty())
            return null;

        Node<T> headNode = _headNode;

        Queue<Node<T>> nodes = new();
        List<Node<T>> nodeList = new();

        nodes.Enqueue(headNode);
        while (nodes.Count != 0)
        {
            Node<T> node = nodes.Dequeue();
            nodeList.Add(node);

            if(node.LeftNode is not null)
                nodes.Enqueue(node.LeftNode);
            if(node.RightNode is not null)
                nodes.Enqueue(node.RightNode);
        }

        return nodeList;
    }

    /// <summary>
    /// 获取二叉树的前序遍历结果（仅包含数据域）
    /// </summary>
    /// <returns>返回结果集</returns>
    /// <exception cref="InvalidOperationException">如果二叉树为空则抛出异常</exception>
    public IEnumerable<T> PreOrderTraversal()
    {
        if(IsEmpty())
            throw new InvalidOperationException("tree is empty");

        return PreOrderTraversalNodes().Select(r => r.Data);
    }

    /// <summary>
    /// 获取二叉树的前序遍历结果（包括数据域和指针域）
    /// </summary>
    /// <returns>返回结果集</returns>
    /// <exception cref="InvalidOperationException">如果二叉树为空则抛出异常</exception>
    private IReadOnlyList<Node<T>> PreOrderTraversalNodes()
    {
        if (IsEmpty())
            throw new InvalidOperationException("tree is empty");

        Node<T> headNode = _headNode;
        List<Node<T>> nodes = new();

        InternalPreOrderTraversalNode(headNode);
        void InternalPreOrderTraversalNode(Node<T>? node)
        {
            if (node is null)
                return;

            nodes.Add(node);
            InternalPreOrderTraversalNode(node.LeftNode);
            InternalPreOrderTraversalNode(node.RightNode);
        }

        return nodes;
    }

    /// <summary>
    /// 获取二叉树的中序遍历结果（仅包含数据域）
    /// </summary>
    /// <returns>返回结果集</returns>
    /// <exception cref="InvalidOperationException">如果二叉树为空则抛出异常</exception>
    public IEnumerable<T> OrderTraversal()
    {
        if(IsEmpty())
            throw new InvalidOperationException("tree is empty");

        return OrderTraversalNodes().Select(r => r.Data);
    }

    /// <summary>
    /// 获取二叉树的中序遍历结果（包含数据域和指针域）
    /// </summary>
    /// <returns>返回结果集</returns>
    /// <exception cref="InvalidOperationException">如果二叉树为空则抛出异常</exception>
    private IReadOnlyList<Node<T>> OrderTraversalNodes()
    {
        if(IsEmpty())
            throw new InvalidOperationException("tree is empty");

        Node<T> headNode = _headNode;
        List<Node<T>> nodes = new();

        InternalOrderTraversalNodes(headNode);
        void InternalOrderTraversalNodes(Node<T>? node)
        {
            if (node is null)
                return;

            InternalOrderTraversalNodes(node.LeftNode);
            nodes.Add(node);
            InternalOrderTraversalNodes(node.RightNode);
        }

        return nodes;
    }

    /// <summary>
    /// 获取二叉树的后序遍历结果（包含数据域）
    /// </summary>
    /// <returns>返回结果集</returns>
    /// <exception cref="InvalidOperationException">如果二叉树为空则抛出异常</exception>
    public IEnumerable<T> PostOrderTraversal()
    {
        if(IsEmpty())
            throw new InvalidOperationException("tree is empty");

        return PostOrderTraversalNodes().Select(r => r.Data);
    }

    /// <summary>
    /// 获取二叉树的后序遍历结果（包含数据域和指针域）
    /// </summary>
    /// <returns>返回结果集</returns>
    /// <exception cref="InvalidOperationException">如果二叉树为空则抛出异常</exception>
    private IReadOnlyList<Node<T>> PostOrderTraversalNodes()
    {
        if (IsEmpty())
            throw new InvalidOperationException("tree is empty");

        Node<T> headNode = _headNode;
        List<Node<T>> nodes = new();

        InternalPostOrderTraversalNodes(headNode);
        void InternalPostOrderTraversalNodes(Node<T>? node)
        {
            if (node is null)
                return;

            InternalPostOrderTraversalNodes(node.LeftNode);
            InternalPostOrderTraversalNodes(node.RightNode);
            nodes.Add(node);
        }

        return nodes;
    }

    /// <summary>
    /// 判断二叉树是否为空
    /// </summary>
    /// <returns>空返回true，否则返回false</returns>
    public bool IsEmpty() => Count == 0;

    /// <summary>
    /// 将二叉树清空
    /// </summary>
    public void Clear()
    {
        _headNode = null!;
        Count = 0;
    }

    public void Dispose()
    {
        Clear();
        GC.SuppressFinalize(this);
    }
}
```

源代码下载地址：<a href = "https://github.com/Altholia/DataStructure/archive/refs/heads/main.zip">download</a>

# 思维导图
![二叉树(Binary Tree).png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202311271007421.png)
