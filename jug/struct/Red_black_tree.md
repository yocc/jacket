# Red–black tree

----

[TOC]



## Overview

[自平衡二叉查找树](https://zh.wikipedia.org/wiki/自平衡二叉查找树)

典型用途是实现[关联数组](https://zh.wikipedia.org/wiki/关联数组)

1972年由[鲁道夫·贝尔](https://zh.wikipedia.org/wiki/鲁道夫·贝尔)发明，被称为"**对称二叉B树**"

黑树的结构复杂，但它的操作有着良好的最坏情况[运行时间](https://zh.wikipedia.org/wiki/算法分析)，并且在实践中高效：它可以在[{\displaystyle {\text{O}}(\log n)}![{\displaystyle {\text{O}}(\log n)}](https://wikimedia.org/api/rest_v1/media/math/render/svg/67697a0b44080bbf967c00d60bf4aac79f9ce385)](https://zh.wikipedia.org/wiki/大O符号)时间内完成查找、插入和删除，这里的{\displaystyle n}![n](https://wikimedia.org/api/rest_v1/media/math/render/svg/a601995d55609f2d9f5e233e36fbe9ea26011b3b)是树中元素的数目



/**
 * 平衡二叉树:就是为了防止二叉搜索树变为线性数据结构,而出现的数据结构
 * 而AVL树-绝对平衡树.左右子树的高度差不能超过1
 * 红黑树:特性:
 * 1.每个结点不是红色就是黑色
 * 2.根节点:一定是黑色的
 * 3.不可能有两个红色的节点连在一起,每个叶子节点都是黑色的空节点(NIl),并且不存储数据
 * 4.每个节点,从该结点到达其可到达的叶子节点的所有路径,都包含相同树目的黑色节点
 * 为什么要用红黑树,
 * 三个操作:
 * 1.变色:
 * 2.左旋: 指针的变化
 * 3.右旋:指针的变化
 * 什么时候左旋?什么时候右旋呢?
 * 所有新加的点一定是红色
 * 红黑树建立的基础就是在二叉查找树的基础之上的.解决了二叉查找树的线性问题;进行平衡性;
 */



![image-20220712161516360](/Users/yocc/Library/Application Support/typora-user-images/image-20220712161516360.png)



## FAQ & troubleshooting





## See Also

https://baijiahao.baidu.com/s?id=1681663951861444437&wfr=spider&for=pc

https://zh.wikipedia.org/wiki/%E7%BA%A2%E9%BB%91%E6%A0%91

https://zhuanlan.zhihu.com/p/143585797