---
title: Leetcode -- Maximum Subarray
date: 2018-04-07 14:02:12
tags: 
- 动态规划
- Leetcode
categories: Algorithm
---

原题链接：https://leetcode.com/problems/maximum-subarray/description/

<!--more-->

##### 题目描述：  求一个给定数组的的子数组的最大和。

这是一道非常经典的动态规划题目，可以归为“局部最优解和全局最优解法”。

基本思路：遍历数组，同时维护两个变量，一个是全局最优解，即到当前元素位置的最优解；一个是局部最优解，即必须包含当前元素的最优解。

接下来是动态规划的递推式：

```cpp
curmax[i] = max(A[i], curmax[i-1]+A[i])
resmax[i] = max(resmax[i-1], curmax[i])
```

局部最优解curmax 一定要包含当前元素，或者只包含当前元素。

获得了考虑了当前元素的局部最优解，那么全局最优解就或者是之前的全局最优，或者变为了现在的当前最优。

如果全局最优不包括当前元素，那么就会被维护在之前的全局最优里面；如果包含当前元素，那么就是局部最优。

注意到curmax和resmax只需要用到一次，因此可以不设置数组，直接用变量替代。

代码：



```cpp
int max(int a, int b){
    return a>b? a: b;
}

int solution(vextor<int>& nums){
    int n = nums.size();
    
    int curmax = nums[0];
    int resmax = nums[0];
    
    for(int i = 1; i < n; i++){
        curmax = max(nums[i], curmax+nums[i]);
        resmax = max(resmax, curmax);
    }
    return resmax;
}

```



