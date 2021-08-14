---
title: 单调栈基础
date: 2021-02-24 10:46:14
tags:
  - Leetcode
  - 单调栈
categories: Algorithm
description: 本文介绍单调栈的基本原理和用法
---

与单调队列类似，对于单调栈来讲，我们不仅要满足栈的后入先出顺序，还要满足栈内元素的单调性，来存储当前栈内元素的对应最大值或最小值。

每次入栈时，先将栈顶所有小于当前值的元素移出，直到当前栈顶元素大于当前值，或者栈为空

例题：

每日温度

![image-20210814111129834](https://gitee.com/MyTypora/typorapic/raw/master/img/20210814112531.png)

代码

```python
class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        length = len(temperatures)
        ans = [0] * length
        stack = []
        for i in range(length):
            temperature = temperatures[i]
            while stack and temperature > temperatures[stack[-1]]:
                prev_index = stack.pop()
                ans[prev_index] = i - prev_index
            stack.append(i)
        return ans
```