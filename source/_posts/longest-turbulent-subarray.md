---
title: Leetcode -- 最长湍流子数组
date: 2021-02-09 11:02:50
tags:
  - Leetcode
  - 动态规划
categories: Algorithm
---

题目链接 ： [Leetcode-978](https://leetcode-cn.com/problems/longest-turbulent-subarray/)

<!--more-->

#### 题目描述

![](https://gitee.com/MyTypora/typorapic/raw/master/20210209110647.png)

#### 解题思路

最长湍流子数组，形象地描述就是数组中的一个子数组，连续数字大小交替变化，我们把这种交替变化用 **上升 下降** 表示，如果`arr[-1]<arr[-2]` 则称下降序列，如果 `arr[-1]>arr[-2]` 则称上升序列，即用最后一个变化方向代表整个序列的变化方向。

考虑动态规划的算法，找到如何描述状态以及状态转移方程。用 `dp[i][0] ` 表示以 `num[i]` 结尾的上升序列的长度；用`dp[i][1]`  表示以`num[i]` 结尾的下降序列的长度。**考虑到上升序列去掉最后一个数字之后成为下降序列，下降序列去掉最后一个数字之后成为上升序列。**

然后我们考虑当前状态 `dp[i][0], dp[i][1]` 与下一个数组`num[i+1]` 之间的大小关系：

- 如果 `num[i+1]>num[i]` 则可以把`num[i+1]`添加到下降序列最后， 成为上升序列；而下降序列长度为1（因为连续两个下降方向）。
- 如果 `num[i+1]<num[i]` 则可以把`num[i+1]`添加到上升序列最后， 成为下降序列；而上升序列长度为1（因为连续两个上升方向）。
- 如果`num[i+1]=num[i]` 则上升序列和下降序列的长度均变为 1。

然后从两个数组中找到最长的那个子数组。

有了状态表示和状态转移方程，就可以写代码解题了。

#### 示例代码

```python 
class Solution:
    def maxTurbulenceSize(self, arr: List[int]) -> int:
        ret = 0
        n = len(arr)
        dp = [[0, 0] for _ in range(n)]
        dp[0][0], dp[0][1] = 1, 1
        for i in range(1, n):
            if (arr[i]<arr[i-1]):
                dp[i][0] = dp[i-1][1]+1
                dp[i][1] = 1
            elif (arr[i]>arr[i-1]):
                dp[i][1] = dp[i-1][0]+1
                dp[i][0] = 1
            else:
                dp[i][0] = 1
                dp[i][1] = 1
        for i in range(n):
            ret = max(ret, dp[i][0])
            ret = max(ret, dp[i][1])

        return ret
```

