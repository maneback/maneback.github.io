---
title:  单调队列基础
date: 2021-02-24 10:30:35
tags:
  - Leetcode
  - 单调队列
categories: Algorithm
description: 介绍单调队列的基本原理与用法
---

单调队列是一个特殊的队列，除满足队列先进先出的特点外，队列内的元素根据需要，还满足单调递增或单调递减。

以单调递增队列为例：当我们想队尾添加元素`x`时，为了保持单调性，要把当前队尾所有小于`x` 的元素从队列中移出，直到队列为空，或是找到了比`x` 大的元素。这样，单调队列头部始终保存的是当前队列内的最大值。

当我们在队列内移除元素时，若当前移除的元素与单调队列头部元素相等，说明此时此最大值在队列内已不存在，此元素也应被移除以维护队列内的最大值。

#### 例题

[剑指offer - 59-II](https://leetcode-cn.com/problems/dui-lie-de-zui-da-zhi-lcof/)

![](https://gitee.com/MyTypora/typorapic/raw/master/20210224104022.png)



#### 代码示例

在这里，我们用一个数组（双向队列）`self.que` 保存所有的队列元素，`self.max_` 保存遇到的最大值。

```python
class MaxQueue:

    def __init__(self):
        self.que = collections.deque()
        self.max_= collections.deque()


    def max_value(self) -> int:
        if not self.max_:
            return -1
        return self.max_[0]

    def push_back(self, value: int) -> None:
        self.que.append(value)
        while self.max_ and self.max_[-1]<value:
            self.max_.pop()
        self.max_.append(value)


    def pop_front(self) -> int:
        if not self.que:
            return -1
        n = self.que[0]
        if self.max_[0] == self.que[0]:
            self.max_.popleft()
        self.que.popleft()
        return n 
```

