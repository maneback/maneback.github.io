---
title: Leetcode - 区间合并
date: 2021-02-24 10:06:25
tags:
  - Leetcode
categories: Algorithm
---

题目链接： [Leetcode-56](https://leetcode-cn.com/problems/merge-intervals/)

<!--more-->

#### 题目描述

![](https://gitee.com/MyTypora/typorapic/raw/master/20210224100900.png)

#### 思路

如果我们按照区间起始位置将所有区间排序，那么可合并的区间在排序后的数组中是连续的。如下图：相同颜色的区间可以被合并。

这样的话只一次遍历区间，并判断每个区间能不能与前一个区间合并。

判断一个区间能不能与前面的区间合并：只需要比较当前区间的起始与前一个区间（可能是已经合并过的区间）的结束位置的大小。

若当前区间在前一个区间结束前开始，则可以合并。

![](https://gitee.com/MyTypora/typorapic/raw/master/20210224100018.png)

#### 代码示例

```python
def merge(intervals):
    intervals.sort(key=lambda x: x[0])
    
    merged = []
    for inter in intervals:
        # cannot be merged
        if not merged or inter[0]>merged[-1][1]:
            # 此区间成为一个新的区间
            merged.append(inter)
        else:
            # 可以合并，取两个区间结束点较大的一个
            merged[-1][1] = max(merged[-1][1], inter[1])
    return merged
```