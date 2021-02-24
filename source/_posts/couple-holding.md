---
title: Leetcode -- 情侣牵手
date: 2021-02-24 10:51:51
tags:
  - 并查集
  - Leetcode
categories: Algorithm
---

题目链接 [Leetcode-765](https://leetcode-cn.com/problems/couples-holding-hands/)

<!--more-->



![image-20210224105405609](https://gitee.com/MyTypora/typorapic/raw/master/20210224105405.png)

#### 解题思路

对于一堆坐错位置的情侣的集合，只要按照首位相连环路交换位置即可，此种交换位置的方法一定是最少的，次数为情侣数量-1。

因此下一步的目标即寻找互相独立的情侣的集合，以及每个集合的情侣数量。

可以用并查集的方法来解决。编号为`n, n+1` 的两个人组成情侣编号为 `n/2`，以此编号来作为节点执行并查集算法

然后利用 map 记录每个并查集集合的大小 $size_i$，再返回 $\sum{size_i-1}$

![image-20210224105550507](https://gitee.com/MyTypora/typorapic/raw/master/20210224105550.png)

#### 代码示例

```python
def minSwapCouple(row):
    n = len(couple)//2
    father = [i for _ in range(n)]
    
    def find(x):
        if father[x] ==x:
            return x
       	f = find(father[x])
        father[x] = f
        return f
   	def union(x, y):
        fx = find(x)
        fy = find(y)
        father[fx] = fy
        
    for i in range(n):
        a, b = row[2*n]//2, row[2*n+1]//2
        union(a, b)
    dit = collections.defaultdict(int)
        for i in range(n):
            f = find(i)
            dit[f]+=1
        ret = 0
        for v in dit.values():
            ret+=(v-1)
        return ret
    
```

