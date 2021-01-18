---
title: Leetcode -- binary tree maximum path sum
date: 2020-06-15 21:04:57
tags: 
  - leetcode
  - tree
  - dfs
categories: 
description: LeetCode-124
---

**题目链接**： [https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/)

#### 题目描述

给定一个**非空**二叉树，返回其最大路径和。

本题中，路径被定义为一条从树中任意节点出发，达到任意节点的序列。该路径**至少包含一个**节点，且不一定经过根节点。

#### 解题思路

首先，这是一道非常标准的树形结构搜索的问题，对于树结构相关的问题，都可以用深度优先搜索或广度优先搜索的方法来解决，此题也不例外。只要考虑好在每个节点上做什么操作、在 child 节点上做什么操作，以及是先序、中序还是后续就行了。

关于这个题，很明显是后序。因为你需要找到当前 node 节点的子节点的路径长，才能确定通过当前 node 节点的路径长。

```python
def dfs(node):
    if not node:
       # pass
    dfs(node.left)
    dfs(node.right)
    # do sth to node
    return #path.
```

对于这个问题，其路径可以不经过 root 节点，那么对于最长路径中的任意一个节点 n，可选的操作有四种：①从 n 的父节点连接 n 到 n 的左子树；②从 n 的父节点连接 n 到 n 的右子树；③从 n 的左子树连接到 n 连接到 n 的右子树；④最大路径从 n 点结束，包括最上的点和最下的点两种情况。

我们用全局变量 `path` 表示当前的全局最优解。

对于前两种情况，到目前为止我们只求得了或者说是遍历过了最长路径的部分，因此这一部分要把经过该节点的子路径的长度返回给父节点继续计算。其中除了情况一和二之外，还有一种就是情况四中的最大路径最下从当前节点结束：即它的所有子树的最大路径都是负值。因此我们要针对这三种情况计算局部最优值。

对于情况三，在得到了左右子树的最大路径值后，都能直接计算出该情况的解，因此此时直接将其与全局最优解比较。此外还可能其其中一个子树的最大子路径值为负数，且父节点的最大子路径值也为负数，即最大路径最上从当前节点结束，都要和全局最优解比较。

具体代码如下：

#### 解题代码

```python
class Solution:
	def maxPathSum(self, root):
        self.path = -1000000
        
        def dfs(node):
            if not node:
                return
           	dl = dfs(node.left)
            dr = node.right
            # local max
            local_max = max(node.val, node.val+dl, node.val+dr)
            self.path = max(node.val+dl+dr, local_max, self.path)
            return local_max
        dfs(root)
        return self.path

```



