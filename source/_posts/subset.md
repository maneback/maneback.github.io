---
title: Leetcode -- subsets
date: 2020-06-09 08:38:42
tags: 
  - backtrack
  - Leetcode
categories: Algorithm
---

原题链接：[https://leetcode-cn.com/problems/subsets/](https://leetcode-cn.com/problems/subsets/)

<!--more-->

##### 题目描述：

给定一组**不含重复元素**的整数数组 *nums*，返回该数组所有可能的子集（幂集）。

**说明：**解集不能包含重复的子集。

##### 示例：

> 输入: nums = [1,2,3]
> 输出:
> [
>   [3],
>   [1],
>   [2],
>   [1,2,3],
>   [1,3],
>   [2,3],
>   [1,2],
>   []
> ]



##### 解法：

本题是非常经典的回溯法解决的题目。

```python
def subset(nums):
    def backtrack(first=0, curr=[]):
        if len(curr)==k:
            output.append(curr)
        for i in range(first, n):
            curr.append(nums[i])
            backtrack(i+1, curr)
            curr.pop()
    output = []
    n = len(nums)
    for k in range(n+1):
        backtrack()
    return output
```

