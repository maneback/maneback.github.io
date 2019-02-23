---
title: Leetcode  --  Maximum product subarray
date: 2018-07-14 12:14:32
tags: 
- 动态规划
- Leetcode
categories: Algorithm
---

原题链接：[https://leetcode.com/problems/maximum-product-subarray/description/](https://leetcode.com/problems/maximum-product-subarray/description/)

<!--more-->

##### 题目描述：求一个给定数组的子数组的最大乘积

这是一个非常经典的动态规划题目，同样可以归为“局部最优解和全局最优解法”。与Maxium subarray 思想类似，但是又有所不同。

回想Maxium subarray， 基本思想是遍历数组，同时维护两个变量，一个是全局最优解，即到当前位置元素的最优解；一个是局部最优解，即必须会包含当前元素的最优解。然后将之前的全局最优解与一定包含当前元素的局部最优解比较，看是否需要更改全局最优解。

同样此题目仍然是采用这种思路，但是由于乘法与加法存在的不同，我们还需要维护局部最小值。

```cpp
public int maxProduct(int[] A){
    if(A == NULL || A.length == 0)
        return 0;
    if(A.length == 1) return A[0];
    
    int max_local = A[0];
    int min_local = A[0];
    int global = A[0];
    
    for(int i = 1; i < A.length; i++){
        int max_copy = max_local; //这个还要用
        max_local = max(max(A[i]*max_local, A[i]), A[i]*min_local);
        min_local = min(min(A[i]*min_local, A[i]), A[i]*max_local);
        global = max(global, max_local);
    }
    return global;
}
```

**每次局部最优解是一定要包括当前元素的，但是全局最优解可能是当前的局部最优解，或者是之前的全局最优解。**

**最优解或者包含当前元素，或者不包含当前元素。**



