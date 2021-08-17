---
title: 背包问题
date: 2021-06-15 19:22:56
tags:
  - 背包问题
categories: Algorithm
description: 背包问题
---



### 问题描述

对于背包问题，是求在具有容量（质量体积个数）约束的情况下，求所有商品满足约束的**可行性解、最优解或解的个数**。

#### 一般化描述

本文关注于 0-1 背包问题，即：每个物品只有一个。

> 物品总数 $n$
>
> 背包容量 $W$
>
> 物品质量 $w_i$
>
> 物品价值 $p_i$

总质量约束为 $W$

物品的质量/成本为 $w_i$

物品的价值为 $p_i$

一般来讲，背包问题可以用动态规划问题来求解。对于背包问题有三类，即：求是否有可行解、最优解的值（价值最大化最小化）、求可行解的总个数。这三类问题对应了三种动态规划递推式。

好多问题都可以转化成背包问题来求解。

对于每一类问题，在这里我们用到动态规划数组 `dp[i][j]` 表示前 $i$ 商品放入容量为 $j$ 的背包时的解。

### 问题分类

#### 最大价值

其转移方程如下：
$$
f[i][j] = \cases{f[i-1][j]\\f[i-1][j-w_i]+p_i}
$$
初始化时，`dp[0][0]=0`。

第一种情况表示不放置第 $i$ 件商品，第二种情况表示放置，所以要给它腾出 $w_i$ 大小的空间。

```python
def bp(weights, profit, W):
    n = len(weights)
    dp = [[0]* (W+1) for _ in range(n+1)]
    for i in range(1, n+1):
        for j in range(profit[i-1], W+1):
            dp[i][j] = max(dp[i-1][j], dp[i-1][j-weights[i-1]]+profit[i-1])
    return dp[n][W]            
```

在这里，我们看到，数组 `dp` 只会用到前面一行来更新，因此我们可以用一个一维的滚动数组来替代二维数组降低空间复杂度。同时在遍历容量时，会用到上一行中小于当前容量的数据，并不会用到大于当前容量的数据。因此要对一维数组对容量进行倒序遍历。



#### 可行解数量

例题： [494 目标和](https://leetcode-cn.com/problems/target-sum/)

> 给你一个整数数组 nums 和一个整数 target 。
>
> 向数组中的每个整数前添加 '+' 或 '-' ，然后串联起所有整数，可以构造一个 表达式 ：
>
> 例如，nums = [2, 1] ，可以在 2 之前添加 '+' ，在 1 之前添加 '-' ，然后串联起来得到表达式 "+2-1" 。
> 返回可以通过上述方法构造的、运算结果等于 target 的不同 表达式 的数目。
>
> 输入：nums = [1,1,1,1,1], target = 3
>
> 输出：5
>
> > 解释：一共有 5 种方法让最终目标和为 3 。
> > -1 + 1 + 1 + 1 + 1 = 3
> > +1 - 1 + 1 + 1 + 1 = 3
> > +1 + 1 - 1 + 1 + 1 = 3
> > +1 + 1 + 1 - 1 + 1 = 3
> > +1 + 1 + 1 + 1 - 1 = 3

数组总和为 `sum`， 设添加 `+` 的数字和为 `positive`, 添加 `-` 的数字和为`negative`，则有一下方程组成立
$$
\cases{positve+negative = sum\\
positive-negative = target}
$$
解方程组可得：
$$
negative = \frac{sum-target}{2}
$$
即从数组中找到若干个数字和为 `negative`，求解的个数。

数字大小即为成本。

 `dp[i]` 表示和为 `i` 的解的个数。

在每一个数字开始循环时，`dp[i]`表示的既是上一轮的解的个数，又是当前轮不包含当前数组的情况下的解的个数。因此再加上包含当前数字的情况的解的个数，即为本轮的解。

初始化为`dp[0] = 1` 其他为0， 表示和为 0 有一种解，即什么都不选。

```python
def findTargetSumWays(nums, target):
    s = sum(nums)
    if (s-target)%2==1 or target>s:
        return 0
   	V = (s-target)//2
    dp = [0]*(V+1)
    dp[0] = 1
    for n in nums:
        for v in range(V, n-1, -1):
            dp[v] += dp[v-n]
    return dp[-1]

	# 二维 dp
    n = len(nums)
    V = (s-target)//2
    dp = [[0]*(V+1) for _ in range(n+1)]
    dp[0][0] = 1
    for i in range(1, n+1):
        num = nums[i-1]
        for j in range(V+1):
            # 因为这第 i 行还没有赋值，下一行可能被用到，所以要从 0  开始遍历。
            dp[i][j] = dp[i-1][j]
            if j>=num:
                dp[i][j]+=dp[i-1][j-num]
	return dp[-1][-1]
            

```



#### 是否存在可行解

> 给你一个 **只包含正整数** 的 **非空** 数组 `nums` 。请你判断是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。
>
> > 输入：[1,5,11,5]
> > 输出：true
> > 解释：数组可以分割成 [1, 5, 5] 和 [11] 。



同样，在每一个数组开始循环前，

```python
def canPatrition(nums):
    s = sum(nums)
    if s%2==1:
        return False
   	V = s//2
    dp = [False]*(V+1)
    for n in nums:
        for v in range(V, n-1, -1):
            dp[v] = dp[v]|dp[v-n]
    return dp[-1]


	# 二维 dp
    n = len(nums)
    V = s//2
    dp = [[False]*(V+1) for _ in range(n+1)]
    dp[0][0] = True
    
    for i in range(1, n+1):
        for j in range(V+1):
            # 因为这第 i 行还没有被赋值，而前面的可能会被下一行用到，所以要从 0 开始赋值。把上一行的复制下来
            dp[i][j] = dp[i-1][j]
            if j>=nums[i-1]:
                dp[i][j] = dp[i][j] | dp[i-1][j-nums[i-1]]
	return dp[-1]
            
```



**参考链接**

- 背包九讲