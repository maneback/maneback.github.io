---
title: Leetcode -- unique binary search trees II
date: 2020-06-16 11:39:50
tags:
  - Leetcode
  - tree
  - dynamic programming
categories: Algorithm
description: LeetCode-95
---

#### 题目描述

给定一个整数 *n*，生成所有由 1 ... *n* 为节点所组成的 **二叉搜索树** 。

0<=n<=8

#### 解题思路



对于一个二叉搜索树中的任一个节点来讲，其左子树的所有节点的值都小于该节点，其右子树的所有节点的值都大于该节点。

对于一个递归的问题，我们只考虑每一步应该做什么，顺序是什么，应该返回什么。

在每一步中，我们只考虑构建一棵子树，然后把子树返回给上一节点。

假设某一步中我们以一个区间`[start, end]` 来构造二叉搜索树，那么对于任意的 `i, start<=i<=end`， 可以以`i`为根节点来开始构造二叉搜索树，它的左子树则是以区间`[start, i-1]`构造的二叉搜索树，它的右子树是以区间`[i+1, end]` 构造的二叉搜索树，就这样递归地构造即可：

```python
def dfs(start, end):
    for i in range(start, end):
        node = Treenode(start, i)
        lc = dfs(start, i-1)
        rc = dfs(i+1, end)
```

但是对于区间 `[start, i-1]` 和`[i+1, end]`构造的左右子树，可能不止为1， 然而其可以任意地组合，这样，我们用一个数组保存以`[start， end]`构造的所有子树，然后再将其返回：

```python
def dfs(start, end):
    if start>end:
        return [None, ]
    res = []
    for i in range(start, end):
        node = Treenode(start, i)
        lc = dfs(start, i-1)
        rc = dfs(i+1, end)
        for l in lc:
            for r in rc:
                node.left = l
                node.right = r
                res.append(node)
    return res
```

这样，我们就完成了代码的书写。

#### 解题代码

```python
def buildTree(n):
    if n==0:
        return []
    def dfs(start, end):
        if start>end:
            return [None, ]
        res = []
        for i in range(start, end):
            node = Treenode(start, i)
            lc = dfs(start, i-1)
            rc = dfs(i+1, end)
            for l in lc:
                for r in rc:
                    node.left = l
                    node.right = r
                    res.append(node)
        return res
   	return dfs(1, n)
    

```

