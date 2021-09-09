---
title: 子序列问题
date: 2021-09-01 10:44:07
tags:
  - 动态规划
  - 字符串
  - Leetcode
categories: Algorithm
---

最长公共子序列

最长上升子序列

摆动序列

编辑距离

<!--more-->

#### 最长公共子序列

题目链接：[LeetCode-1143](https://leetcode-cn.com/problems/longest-common-subsequence/)

##### 题目描述

> 给定两个字符串 `text1` 和 `text2`，返回这两个字符串的最长 **公共子序列** 的长度。如果不存在 **公共子序列** ，返回 `0` 。
>
> 一个字符串的 **子序列** 是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。
>
> - 例如，`"ace"` 是 `"abcde"` 的子序列，但 `"aec"` 不是 `"abcde"` 的子序列。
>
> 两个字符串的 **公共子序列** 是这两个字符串所共同拥有的子序列。

##### 分析

我们用一个二维dp数组 `dp[i][j]` 表示 `text1[0,i]` 和 `text2[0,j]` 的最长公共子序列，则根据最后一个字符 `text1[i]` 和`text2[j]` 的关系，得到下面的状态转移方程：
$$
dp[i][j] = \cases{dp[i-1][j-1]+1,\;\;\; text1[i]=text2[j],\\
				\max\{dp[i][j-1], dp[i-1][j]\}, \;\;text1[i]\neq text2[j]}
$$

##### 代码

```java
public int longestCommonSubsequence(String text1, String text2) {
    int m = text1.length();
    int n = text2.length();
    int[][] dp = new int[m+1][n+1];
    for(int i=1; i<=m; i++){
        for(int j=1; j<=n; j++){
            if(text1.charAt(i-1)==text2.charAt(j-1)){
                dp[i][j] = dp[i-1][j-1]+1;
            }
            else{
                dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1]);
            }
        }
    }
    return dp[m][n];
}
    
```

#### 最长上升子序列

题目链接： [LeetCode-300](https://leetcode-cn.com/problems/longest-increasing-subsequence/)

##### 题目描述

> 给你一个整数数组 nums ，找到其中最长严格递增子序列的长度。
>
> 子序列是由数组派生而来的序列，删除（或不删除）数组中的元素而不改变其余元素的顺序。例如，[3,6,2,7] 是数组 [0,3,1,6,2,2,7] 的子序列。

##### 分析

我们用一个一维的dp数组 `dp[i]` 表示 数组`nums[0...i]` 的严格递增子序列的长度。考虑到子问题：对于一个严格递增子序列，去掉其最后一个元素，其仍然是一个严格递增子序列。所以，有状态转移方程
$$
dp[i] = \max_{j<i\and nums[j]<nums[i]}(dp[j]+1)
$$

##### 代码

```java
public int lengthOfLIS(int[] nums) {
    int n = nums.length;
    int[] dp = new int[n];
    dp[0] = 1;
    for(int j=0; j<n; j++){
        for(int i=0; i<j; i++){
            if(nums[i]<nums[j]){
                dp[j] = Math.max(dp[j], dp[i]+1);
            }
        }
    }
    return dp[n-1];
}
```

#### 摆动序列

题目链接 [LeetCode-376](https://leetcode-cn.com/problems/wiggle-subsequence/)

##### 题目描述

> 如果连续数字之间的差严格地在正数和负数之间交替，则数字序列称为 摆动序列 。第一个差（如果存在的话）可能是正数或负数。仅有一个元素或者含两个不等元素的序列也视作摆动序列。
>
> 例如， [1, 7, 4, 9, 2, 5] 是一个 摆动序列 ，因为差值 (6, -3, 5, -7, 3) 是正负交替出现的。
>
> 相反，[1, 4, 7, 2, 5] 和 [1, 7, 4, 5, 5] 不是摆动序列，第一个序列是因为它的前两个差值都是正数，第二个序列是因为它的最后一个差值为零。
> 子序列 可以通过从原始序列中删除一些（也可以不删除）元素来获得，剩下的元素保持其原始顺序。
>
> 给你一个整数数组 nums ，返回 nums 中作为 摆动序列 的 最长子序列的长度 。

##### 分析

只要理解摆动序列的概念，画个图的话，就是：

![](https://gitee.com/MyTypora/typorapic/raw/master/img/20210902192124.png)

这个图中表示了八个数据点，即每一个波峰波谷处。然而原始的数组，可能每一个上山或下山的过程中，都还存在这不止一个数据点。那么其实我们要找到的，就是这些位于波峰波谷的点。

定义`d[i]=sign(nums[i]-nums[i-1])` 即表示是上坡还是下坡。每当遇到`d[i]!=d[i-1]`的情况，则表明遇到了波峰或者波谷，此时把这个数字添加到递增子序列中（表现子序列长度加一）。其次，数组中可能会存在连续的相同数字，所以要用一个 `flag` 表示它是否在动了。

##### 代码

```java
public int wiggleMaxLength(int[] nums) {
    int n = nums.length;
    if(n<2){
        return n;
    }
    boolean d = false;
    boolean flag = false;
    int count = 1;
    for(int i=1; i<n; i++){
        if(nums[i]==nums[i-1]){
            continue;
        }
        boolean cd = nums[i]>nums[i-1];
        if(!flag|| d!=cd){
            count++;
        }
        d = cd;
        flag=true;
    }
    return count;
}
```

#### 编辑距离

题目链接 [LeetCode-72](https://leetcode-cn.com/problems/edit-distance/)

##### 题目描述

> 给你两个单词 `word1 `和 `word2`，请你计算出将 `word1 `转换成 `word2 `所使用的最少操作数 。
>
> 你可以对一个单词进行如下三种操作：
>
> 插入一个字符
> 删除一个字符
> 替换一个字符

##### 分析

很显然，两个字符串的问题，继续使用二维的dp数组，关键之处在于理解这个数组表示的含义。以及状态转移方程。

在第一个问题公共子序列中，我们用`dp[i][j]` 表示了前缀 `s1[0...i]` 和 `s2[0...j]` 的公共子序列。同理在这个问题中也是类似。

那么状态转移方程应该怎么变， 是与字符串`s1[i]`和字符`s2[j]`相关的。

相等时可以直接跳过不用操作。 `dp[i][j] = dp[i-1][j-1]`

不相等时，考虑对字符串`s1` 的三种操作：

- 删除字符`s1[i]` ，相当于子问题 `dp[i-1][j]` 再多一步删除的操作
- （在A中插入字符`s2[j]`）。 相当于让新插入的字符去匹配`s2[j]`，在子问题`dp[i][j-1]`上增加一步插入操作
- 修改字符`s1[i]`。相当于子问题`dp[i-1][j-1]` 上增加一步修改操作。

其实要理解这个问题的dp数组。把每一个 `dp[i][j]`都理解成这个矩阵的**最后**一个元素，即完全不要管后面位置的元素，可以帮助理解

画个图来理解一下这三种情况，框中为此次操作后的相同字符串。

![](https://gitee.com/MyTypora/typorapic/raw/master/img/20210903211319.png)

##### 代码

```java
public int minDistance(String word1, String word2){
    int n1 = word1.length();
    int n2 = word2.length();
    if(n1==0 || n2==0){
        return n1+n2;
    }
    int[][] dp = new int[n1+1][n2+1];
    for(int i=1; i<=n1; i++) dp[i][0] = i;
    for(int i=1; i<=n2; i++) dp[0][i] = i;
    for(int i=1; i<=n1; i++){
        for(int j=1; j<=n2; j++){
            if(word1.charAt(i-1)==word2.charAt(j-1)){
                dp[i][j] = dp[i-1][j-1];
            }
            else{
                dp[i][j]= Math.min(Math.min(dp[i-1][j], dp[i][j-1]), dp[i-1][j-1])+1;
            }
        }
    }
    return dp[n1][n2];

}
```



