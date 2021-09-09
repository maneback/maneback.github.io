---
title: 回溯算法（二）
date: 2021-08-25 14:50:18
tags:
  - 回溯算法
  - Leetcode
categories: Algorithm
description: 继续几个回溯法的例题
---

之前的一篇[文章](../back/index.html)介绍了四个跟数组相关的回溯法例题，子集问题与全排列问题，这次继续介绍另外一种回溯法的题。

#### 例题1 路径总和

链接 [Leetcode-112](https://leetcode-cn.com/problems/path-sum/)

> 给你二叉树的根节点 root 和一个表示目标和的整数 `targetSum `，判断该树中是否存在 根节点到叶子节点 的路径，这条路径上所有节点值相加等于目标和 `targetSum `。
>
> 叶子节点 是指没有子节点的节点。
>
> ![img](https://gitee.com/MyTypora/typorapic/raw/master/img/20210825145405.jpeg)

这个就更简单了，连搜索树都帮我们建好了，而且只是一个二叉树。只需要思考：什么样的答案是满足条件的解。

我们可以在二叉树的后序遍历中，记录路径和，到子节点判断是否和等于 `targetSum`，再返回 `True` 或是 `False`。

另外，题目中并没有明确说明，所有节点都为非负数，所以我们并不能进行剪枝。要是加上这个条件的话，可以在前缀和已经大于 `targetSum` 的条件下，可以提前返回，不必再继续去搜索子树。

```python
class Solution:
    def hasPathSum(self, root: TreeNode, sum: int) -> bool:
        def dfs(node, cursum):
            if not node.left and not node.right: # 到根节点判断和是否相等
                if cursum==node.val:  
                    return True
                else:
                    return False
            if node.left and node.right:
                return dfs(node.left, cursum-node.val) or dfs(node.right, cursum-node.val)
            elif node.left and not node.right:
                return dfs(node.left, cursum-node.val)
            elif not node.left and node.right:
                return dfs(node.right, cursum-node.val)
        if not root:
            return False
        return dfs(root, sum)
```

#### 例题2 路径总和II

链接 [Leetcode-113](https://leetcode-cn.com/problems/path-sum-ii/)

> 给你二叉树的根节点 root 和一个整数目标和 targetSum ，找出所有 从根节点到叶子节点 路径总和等于给定目标和的路径。
>
> 叶子节点 是指没有子节点的节点。

第一个题是判断是否存在，这个题是找出所有，那其他都一样嘛，用一个数组`res`记录答案，然后在遍历中不仅记录路径和，还要记录路径的所有节点值。

在叶子结点不返回 `True` 或`False`。而是把 `trace` 加到`res` 中。

```python
class Solution:
    def pathSum(self, root: TreeNode, sum: int) -> List[List[int]]:

        self.res = []
        if not root:
            return self.res
        
        def dfs(node, pre, summ):
            if not node.left and not node.right:
                if summ==node.val:
                    self.res.append([x for x in pre]) # 给 res 添加一个解
            if node.left:
                dfs(node.left, pre+[node.left.val], summ-node.val)
            if node.right:
                dfs(node.right, pre+[node.right.val], summ-node.val)
                
        
        dfs(root, [root.val], sum)
        return self.res
```

#### 例题3 组合总数

链接 [Leetcode-39](https://leetcode-cn.com/problems/combination-sum/)

> 给定一个无重复元素的正整数数组 `candidates `和一个正整数 `target `，找出 `candidates `中所有可以使数字和为目标数 `target `的唯一组合。
>
> `candidates `中的数字可以**无限制重复被选取**。如果至少一个所选数字数量不同，则两种组合是唯一的。 
>
> 对于给定的输入，保证和为 `target `的唯一组合数少于 150 个。
>

我为何要把这个组合总数与路径和放在一起。

路径和是人家给定了你一个二叉树，想象一下，假如 `candidates ` 中有 `n` 个数字，是不是能转换成一个 `n`层的`n` 叉树？当然我们并不会真的去建一棵树，只是在搜索的时候把数组**看成** 是一棵树。

这个并不是要求搜索到 **叶节点** ，即在任意一个节点都可能往 `res` 集合中更新一个答案。

但是直接建树的话，树的规模一定比上面的二叉树大很多。同时我上面又提到过，要是给了条件说，数组中全为正数，那么其实是可以提前剪枝的，从而缩小搜索空间。

```python
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        size = len(candidates)
        if size==0:
            return []
        candidates.sort()
        path = []
        self.res = []

        self.dfs(candidates, 0, path, target)
        return res
        
    def dfs(self, candidates, begin, path, target):
        if target == 0:
            self.res.append(path[:])
            return 
        
        for index in range(begin, len(candidate)):
            residue = target-candidates[index]

            if residue<0:
                break # 提前终止
            path.append(candidates[index])
            self.dfs(candidates, index, path, residue) # 数字可重复使用，因此下一层继续从 index 开始。
            path.pop()
```

#### 例题4 组合总和II

链接 [Leetcode-40](https://leetcode-cn.com/problems/combination-sum-ii/)

> 给定一个数组 `candidates` 和一个目标数 `target` ，找出 `candidates `中所有可以使数字和为 `target `的组合。
>
> `candidates `中的每个数字在每个组合中只能使用一次。
>
> **注意**：解集不能包含重复的组合。 

这个只是加上了条件，只能使用一次。那么再回忆一下在上一篇中，**子集**中的做法，保证每一个解中数字的添加顺序等同于数字在原数组中的顺序即可，即树中的下一层节点仅包含当前节点之后的数字。

```python 
class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        self.res = []
        

        def bp(trace, idx):
            if sum(trace)== target:
                # print(idx, trace)
                self.res.append([ _ for _ in trace])
                return 
            elif sum(trace)>target:
                return 
            for i in range(idx, len(candidates)):
                if i>0 and candidates[i]==candidates[i-1] and not used[i-1]:
                    continue
                used[i] = True
                trace.append(candidates[i]) 
                bp(trace, i+1) # 下一层仅包含当前 i  之后的数字
                used[i] = False
                trace.pop()

        used = [False]*len(candidates)
        candidates = sorted(candidates)
        # print(candidates)
        bp([], 0)
        return self.res
```

