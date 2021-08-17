---
title: 滑动窗口
date: 2021-06-14 21:29:42
tags:
  - 滑动窗口
categories: Algorithm
description: 滑动窗口
---



#### 滑动窗口

滑动窗口可以用来解决一系列的数组问题，如最长无重复子数组（子串）、窗口最大值、满足某种条件的最长数组/最短数组。

感觉滑动窗口可以分为几类：

1. 定长窗口
2. 寻找最长窗口
3. 寻找最短窗口

其主要的区别就在于：如何添加元素、合适进行窗口的合理性判断（即判断是否满足条件）以及更新答案。

对于定长窗口来讲，只要一直保持窗口的长度，每次增加一个元素并删除一个元素即可。

对于最长窗口来讲，每次删除一个最左元素，然后一直扩展右边界，直到不满足条件（窗口内仍满足条件），再更新答案。

对于最短窗口来讲，每次一直扩展右边界，直到不满足条件，然后再一直删除左元素，在循环中更新答案。（因为是寻找最短窗口，且在缩短左边界，因此这种情况下最后一次更新答案一定是最小值。）

#### 存在重复元素II

题目链接 [Leetcode-219](https://leetcode-cn.com/problems/contains-duplicate-ii/)

- 题目描述

给定一个整数数组和一个整数 k，判断数组中是否存在两个不同的索引` i` 和 `j`，使得 `nums [i] = nums [j]`，并且 `i `和` j` 的差的 绝对值 至多为 `k`。

- 分析

最初我是用一个dict 保存每个数字出现的次数，每次用滑动窗口添加元素删除元素，但是后来边界样例给我惊了，那就是当`k`的大小等于数组的长度的时候，是没有办法判断的。所以不能这样搞。

后来直接用字典记录每个数字最后出现的位置，再用位置去相减判断距离就好了。

- 错误代码

```python
from collections import defaultdict

class Solution:
    def containsNearbyDuplicate(self, nums, k): 
        windows = defaultdict(int)
        for i in range(k):
            windows[nums[i]]+=1
        for i in range(k, len(nums)):
            if windows[nums[i]]>0:
                return True
            else:
                windows[nums[i-k]]-=1
                windows[nums[i]]+=1
        return False
```

- 正确代码

```python
class Solution:
    def containsNearbyDuplicate(self, nums: List[int], k: int) -> bool:       
        dit = {}
        for idx, n in enumerate(nums):
            if n in dit and idx-dit[n]<=k:
                return True
            else:
                dit[n] = idx
        return False
```

#### 爱生气的书店老板

题目链接 [Leetcode-1052](https://leetcode-cn.com/problems/grumpy-bookstore-owner/)

- 题目描述

![image-20210810140952301](https://gitee.com/MyTypora/typorapic/raw/master/img/20210810140952.png)

- 分析

当已知有 `grumpy` 数组时候，对于不生气的情况，无论怎么怎么控制情绪，这部分满意的顾客总数是不会改变的，所以我们只需要考虑，老板控制了脾气之后，最大能 improve 多少挽回多少新的顾客。如果把 `grumpy` 与 `customer` 数组做 `element-wise` 乘积的话，就可以知道能够挽回的顾客数量，再进行 `X` 长度的子数组和取最大就好了。

```python
class Solution:
    def maxSatisfied(self, customers: List[int], grumpy: List[int], X: int) -> int:
        n = len(customers)
        level = sum([c*(1-g) for c, g in zip(customers, grumpy)])

        inc = 0
        for i in range(X):
            inc+= customers[i]*grumpy[i]
        max_inc = inc

        for i in range(X, n):
            inc = inc+customers[i]*grumpy[i]-customers[i-X]*grumpy[i-X]
            max_inc = max(inc, max_inc)
        return level + max_inc
        
```

#### 无重复字符的最长子串

给定一个字符串 `s` ，请你找出其中不含有重复字符的 **最长子串** 的长度。

- 分析

**依次枚举子串起始位置的左边界**，记录以该位置开始子串的最长长度，每一次剔除一个最左字符串， 不断地扩展右边界。直到不满足条件位置，这时判断是否需要更新答案。

**窗口内永远是满足条件的子串，而非剔除直到不满足。**

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        ccc = set()
        n = len(s)
        rk, ans = -1, 0
        for i in range(n):
            if i!=0:
                ccc.remove(s[i-1])
            while rk+1<n and s[rk+1] not in ccc:
                ccc.add(s[rk+1])
                rk += 1
            ans = max(ans, rk+1-i)
        return ans
```

#### 滑动窗口最大值

- 题目描述

给你一个整数数组 nums，有一个大小为 k 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 k 个数字。滑动窗口每次只向右移动一位。

返回滑动窗口中的最大值。

- 分析

只要用 滑动窗口 +  单调队列 即可解决，每次边界移动一个位置，向结果数组中添加一个元素。

- 代码

```python
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        if k==1:
            return nums
        que = collections.deque()
        res = []
        n = len(nums)
        # init 
        for i in range(k):
            while que and que[-1]<nums[i]:
                que.pop()
            que.append(nums[i])
        res.append(que[0])
        for i in range(k, n):
            
            if que[0]==nums[i-k]:
                que.popleft()
            while que and que[-1]<nums[i]:
                que.pop()
            que.append(nums[i])
            res.append(que[0])
        return res 
```

#### 长度最小的子数组

- 题目描述

给定一个含有 `n` 个正整数的数组和一个正整数` target` 。

找出该数组中满足其和 `≥ target` 的长度最小的 连续子数组 `[numsl, numsl+1, ..., numsr-1, numsr]` ，并返回其长度。如果不存在符合条件的子数组，返回 `0` 。

- 分析

返回最小长度，依旧是扩充右边界，判断是否满足，缩减左边界

因为是最小长度，所以在缩减左边界时候更新答案

注意当退出内层 `while` 循环时，其实已经不满足和 `>= target` 的条件了，所以要在 `while` 循环内部更新答案。窗口是在不断减小的，所以最后一次更新一定是最小的，不会影响结果的正确性。

```python
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        if sum(nums)<target:
            return 0
        if sum(nums)==target:
            return len(nums)
        n = len(nums)
        left, right = 0, 0
        wind_sum = 0
        res = n+1
        while right<n:
            wind_sum += nums[right]
            while wind_sum>=target:
                res = min(res, right-left+1)
                wind_sum-=nums[left]
                left+=1
            right += 1
        return 0 if res==n+1 else res
```