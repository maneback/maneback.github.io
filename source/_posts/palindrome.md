---
title: 回文串
date: 2021-08-29 19:32:55
tags:
  - 动态规划
  - 回文串
  - Leetcode
categories: Algorithm
---

最长回文子串

回文字符串的个数

最长回文子序列

分割回文串

<!--more-->

#### 最长回文子串

题目链接： [LeetCode-5](https://leetcode-cn.com/problems/longest-palindromic-substring/)

##### 题目描述

> 给你一个字符串 `s`，找到 `s` 中最长的回文子串。

##### 分析

题目描述很简单，也很直白。

一般来讲，字符串，尤其是回文字符串的问题，都会与动态规划相关，此题也不例外。

首先，先考虑如何用动态规划，找出最长回文子串的**长度**，即先不要想子串到底是什么。

使用动态规划的关键则在于，如何找到最优子结构。

对于一个字符串（长度大于2）来讲，如果它是一个回文串，那么其去掉首尾的两个字母后仍是一个回文串。而边界条件即为长度为`1` 或者 `2` 的情况。

我们用一个布尔类型的二维数组 `dp[i, j]`  表示 子串 `s(i,j)` 是否是一个回文串，那么可以得到如下的状态转移方程：
$$
dp[i,j] = dp[i+1,j-1]\and S_i==S_j
$$
在边界条件中，对于任何一个长度为1的字符串，都可以认为它是一个回文串；对于任何一个长度为2的字符串，只要两个字母相等，它就是一个回文串，即边界条件为：
$$
\cases{dp[i,i]=true\\dp[i,i+1]=(S_i==S_{i+1})}
$$


同时考虑到 `i,j` 的大小关系一定有 `i<=j`， 因此我们只需要来填写dp数组的上半部分。

然后再考虑如何从边界条件开始扩展：可以按照字符串的长度来扩展，即每次不断增加字符串的长度。

这样的话，每找到一个子串，就可以记录它的长度和起始位置，最终根据起始位置和长度返回子串即可。

##### 代码

代码如下：

```java
public  String longestPalindrome(String s) {
    int n = s.length();
    if(n<2){
        return s;
    }
    int res = 1;
    int begin = 0;
    boolean[][] dp = new boolean[n][n];
    for(int i=0; i<n; i++){
        dp[i][i] = true;
    }
    char[] strArray = s.toCharArray();

    for(int l = 2; l<=n; l++){
        for(int left=0; left<n; left++){
            int right = left + l -1;
            if(right>=n){
                break;
            }
            if(strArray[left]!=strArray[right]){
                dp[left][right] = false;
            }
            else{
                if(l==2){
                    dp[left][right] = true;
                }
                else{
                    dp[left][right] = dp[left+1][right-1];

                }
            }
            if(dp[left][right] && right-left+1>res){
                res = right-left+1;
                begin = left;
            }
        }
    }
    return s.substring(begin, begin+res);

}
```

- 第二种遍历方法：

仔细观察状态转移方程，发现每一个 `dp[i,j]` 只会用到在矩阵中其左下方位置的元素，所以，我们还可以倒序遍历 `left`。

```java
public  String longestPalindrome(String s) {
        int n = s.length();
    if(n<2){
        return s;
    }
    int res = 1;
    int begin = 0;
    boolean[][] dp = new boolean[n][n];
    for(int i=0; i<n; i++){
        dp[i][i] = true;
    }
    char[] strArray = s.toCharArray();

    for(int left=n-1; left>=0; left--){
        for(int right=left+1; right<n; right++){
            dp[left][right] = dp[left+1][right-1]&&(strArray[left]==strArray[right]);
            if(dp[left][right] && right-left+1>res){
                res = right-left+1;
                begin = left;
            }
        }
    }
    return s.substring(begin, begin+res);
    }
```



其实这两种原理是一样的，都是字符串长度由短向长的遍历。

第一种遍历是以**子串长度**优先，先遍历所有短的字符串，再遍历长的；第二种是从以**子串起始位置**优先，从结尾处开始遍历。画个图表示就是：



![](https://gitee.com/MyTypora/typorapic/raw/master/img/20210902165025.png)



#### 回文字符串的个数

题目链接：

- [LeetCode-647](https://leetcode-cn.com/problems/palindromic-substrings/)
- [Offer II-020](https://leetcode-cn.com/problems/a7VOhD/)

##### 题目描述

> 给定一个字符串 `s` ，请计算这个字符串中有多少个回文子字符串。
>
> 具有不同开始位置或结束位置的子串，即使是由相同的字符组成，也会被视作不同的子串。

##### 题目分析

按照上一个题的思路，使用动态规划找到所有的回文字符串。然后计数。

##### 代码

```java
public int countSubstrings(String s) {
    int n = s.length();
    boolean[][] dp = new boolean[n][n];
    int ans = n;
    for(int i=0; i<n; i++){
        dp[i][i] = true;
    }
    for(int left=n-1; left>=0; left--){
        for(int right=left+1; right<n; right++){
            if(right-left==1){
                dp[left][right] = (s.charAt(left)==s.charAt(right));
            }
            else{
                dp[left][right] = dp[left+1][right-1]&&(s.charAt(left)==s.charAt(right))
            }
            if(dp[left][right]){
                ans++;
            }
        }
    }
    return ans;
}
```





#### 最长回文子序列

题目链接： [LeetCode-516](https://leetcode-cn.com/problems/longest-palindromic-subsequence/)

##### 题目描述

> 给你一个字符串 s ，找出其中最长的回文子序列，并返回该序列的长度。
>
> 子序列定义为：不改变剩余字符顺序的情况下，删除某些字符或者不删除任何字符形成的一个序列。

##### 分析

题目描述如上面那个题一样简单。

仍然利用上面动态规划的思路。上面我们用一个布尔类型的数组来表示一个字符串是否为回文子串。但是子序列与子串的区别在于，子序列可以是不连续的，什么意思呢？

举个例子：

- 在判断回文子串`s(i,j)` 时，一定是与字符$S_{i+1}$ 和 $S_{j-1}$完全相关，回文子串中一定会包括这两个字符。
- 但是在判断回文子序列`s(i,j)` 时，子序列中不一定非得要包含$S_{i+1}$ 和 $S_{j-1}$ 这两个字符，可能只包含其中一个，或是一个都没有。

也就是说，我们无法用一个布尔类型的数组来记录，这无法给我们提供一个完整的信息。

既然布尔类型的不够用，就直接记录长度呗。注意这里又有一个不同了：

- 在构造回文子串时，两个字符$S_{i}$ 和 $S_{j}$ 一定是同时添加的，如果它们两个不相等，则无法构成回文串。
- 在构造回文子序列时，如果它们两个不相等，我们还可以尝试添加其中一个。

状态转移方程如下：
$$
dp[i,j] = \cases{dp[i+1][j-1]+2, S_i==S_j\\
				\max(dp[i][j-1], dp[i+1][j]), S_i\neq S_j)}
$$
以及边界条件仍然是长度为1 的子串
$$
dp[i,i]=1
$$
由于状态转移方程都是从长度较短的子序列向长度较长的子序列转移，因此需要注意动态规划的循环顺序。

最终得到`dp[0][n-1]`即为字符串的最长回文子序列的长度。

##### 代码

```java
public int longestPalindromeSubseq(String s) {
    int n = s.length();
    if(n<2){
        return n;
    }
    char[] charArray = s.toCharArray();
    int[][] dp = new int[n][n];
    for(int i=0; i<n; i++){
        dp[i][i] = 1;
    }
    for(int left=n-2; left>=0; left--){
        for(int right=left+1; right<n; right++){
            if( charArray[left]==charArray[right]){
                dp[left][right] =dp[left+1][right-1]+2;
            }
            else{
                dp[left][right] = Math.max(dp[left+1][right], dp[left][right-1]);
            }
        }
    }
    return dp[0][n-1];


}
```



#### 分割回文串

题目链接： [LeetCode-131](https://leetcode-cn.com/problems/palindrome-partitioning/)

##### 题目描述

> 给你一个字符串 `s`，请你将 `s` 分割成一些子串，使每个子串都是 **回文串** 。返回 `s` 所有可能的分割方案。
>
> **回文串** 是正着读和反着读都一样的字符串。

##### 题目分析

首先，在第一个题目中，已经知道了如何用动态规划算法，判断并记录字符串中任意一个子串是否为回文串，得到这个之后，只要再用回溯法来尝试每一个可行的分割位置就行了。

再来回忆一下[回溯法]()。每个可能的回文串结尾的位置，都是一个可被选的分割点。

##### 代码

```java
boolean[][] dp;
    List<List<String>> res = new ArrayList<List<String>>();
    List<String> ans = new ArrayList<>();
    int n;
    public List<List<String>> partition(String s) {
        n = s.length();
        dp = new boolean[n][n];
        for(int i=0; i<n; i++){
            dp[i][i] = true;
        }
        for(int l=n-1; l>=0; l--){
            for(int r=l+1; r<n; r++){
                if(r-l==1){
                    dp[l][r] = (s.charAt(l)==s.charAt(r));
                }
                else
	                dp[l][r] = dp[l+1][r-1]&&(s.charAt(l)==s.charAt(r));
            }
        }
        dfs(s, 0);
        return res;
    }
    public void dfs(String s, int index){
        if(index==n){
            res.add(new ArrayList<String>(ans));
            return;
        }
        for(int ch=index; ch<n; ch++){
            if(dp[index][ch]){
                ans.add(s.substring(index, ch+1));
                dfs(s, ch+1);
                ans.remove(ans.size()-1);
            }
        }
    }
```





#### 分割回文串 II

题目链接：[LeetCode-132](https://leetcode-cn.com/problems/palindrome-partitioning-ii/)

##### 题目描述

> 给你一个字符串 `s`，请你将 `s` 分割成一些子串，使每个子串都是回文。
>
> 返回符合要求的 **最少分割次数** 。

##### 题目分析

与第一个分割回文串的问题相同，第一步也是找出并记录字符串的回文子串供第二步使用。

其实对于第二步，也可以采用上述的方法，找出所有可能的分割方案，然后选择一个分割次数最小的。但是这么做太麻烦了。

第二步中仍使用动态规划来求解，最少分割次数。

设`dp[i]` 表示字符串$S_0...S_i$  的最少分割次数，假设已经已知了`dp[i]` 的分割方案，对于任意的`j>i`，如果$S_{i+1}...S_j$ 是一个回文子串，那么 $S_0...S_j$ 的分割方案，可以在 $S_0...S_i$ 分割的基础上，再在 $S_i, S_{i+1}$ 两个字符间多一次分割，即：
$$
dp[j] = \min_{i<j}\{dp[i]\}+1, \mbox{ if } S_{i+1}...S_j \mbox{ is Palindrome}
$$


##### 代码

```java
public int minCut(String s) {
    int n = s.length();
    boolean[][] dp =  new boolean[n][n];
    for(int i=0; i<n; i++){
        dp[i][i] = true;
    }
    char[] strArray = s.toCharArray();
    for(int left=n-1; left>=0; left--){
        for(int right=left+1; right<n; right++){
            if(right-left==1){
                dp[left][right] = (strArray[left]==strArray[right]);
            }
            else
                dp[left][right] = dp[left+1][right-1]&&(strArray[left]==strArray[right]);
        }
    }
    int[] f = new int [n];
    f[0] = 0;
    for(int i=0; i<n; i++){
        f[i] = n+1;
        if(dp[0][i]){
            f[i] = 0;
        }
        else{
            for(int j=0; j<i; j++){
                if(dp[j+1][i]){
                    f[i] = Math.min(f[j]+1, f[i]);
                }

            }
        }
    }
    return f[n-1];
}
```

#### 分割回文串 III

题目链接：[LeetCode-1278](https://leetcode-cn.com/problems/palindrome-partitioning-iii/)

##### 题目描述

> 给你一个由小写字母组成的字符串 `s`，和一个整数 `k`。
>
> 请你按下面的要求分割字符串：
>
> - 首先，你可以将 `s` 中的部分字符修改为其他的小写英文字母。
> - 接着，你需要把 `s` 分割成 `k` 个非空且不相交的子串，并且每个子串都是回文串。
>
> 请返回以这种方式分割字符串所需修改的最少字符数。

##### 题目分析

这道题目同样使用动态规划。

首先定义动态规划数组，用`dp[end][j]` 表示前`end`个字符，分割成`j`个非空不相交的回文串，需要最小修改的字符数。

如我们已经已知了如何将前`end`个字符串分割成`j-1` 个子串，那么下一步便是考虑如何将它分成`j`个子串。

其实只需要枚举第`j`个子串的起始位置`ij`，把 `S[0,ij-1]`分割成`j`个子串，然后 `S[ij, end]`作为第`j`个子串。

然后再计算子串 `S[ij, end]`构成回文串需要修改的字符数量--定义为 `cost(ij, end)`。

同时，为了得到非空的字符串，还要有以下两个限制：

- `j<=end` 因为前 `end` 个字符，最多能被分成 `end` 个子串。
- `ij>=j-1`。 因为要得到`j-1`个非空子串，前面至少要有这么多个字符。

$$
dp[end][j] = \min_{ij}\{dp[ij][j-1] +cost(ij, end)\}
$$

边界条件为：当`j=1`时，也就是不需要分割时，则有`dp[i][1] = cost[0][i]`成立。



为了避免重复计算，我们可以再用一次动态规划来记录所有的 `cost(ij, end)`。这个动态规划的边界条件即为：对任意长度为1的字符串，都有`cost(i,i)=0`。之后类似于寻找最长子串，以字符串长度为优先遍历，得到：
$$
cost(i,j)=\cases{cost(i+1,j-1)+1,\;\; S_i\neq S_j\\
cost(i+1, j-1),\;\;\;\; S_i=S_j}
$$

##### 代码

```java
public int palindromePartition(String s, int k){
    int n = s.length();
    int[][] cost = new int[n][n];

    for(int l=2; l<=n; l++){
        for(int left=0; left<n; left++){
            int right = left+l-1;
            if(right>=n){
                break;
            }
            cost[left][right] = cost[left+1][right-1];
            if(s.charAt(left)!=s.charAt(right)){
                cost[left][right]++;
            }
        }
    }
    int[][] f = new int[n+1][k+1];
    for(int i=0; i<=k; i++){
        Arrays.fill(f[i], 10086);
    }
    f[0][0] = 0;
    for(int i=1; i<=n; i++){
        for(int j=1; j<=Math.min(k, i); j++){
            if(j==1){
                f[i][j] = cost[0][i-1];
            }
            else{
                for(int i0=j-1; i0<i; i0++){
                    f[i][j] = Math.min(f[i][j], f[i0][j-1]+cost[i0][i-1]);
                }
            }
        }
    }
    return f[n][k];

}
```











#### 分割回文串 IV

题目链接：[LeetCode-1745](https://leetcode-cn.com/problems/palindrome-partitioning-iv/)

##### 题目描述

> 给你一个字符串 `s` ，如果可以将它分割成三个 **非空** 回文子字符串，那么返回 `true `，否则返回 `false `。
>
> 当一个字符串正着读和反着读是一模一样的，就称其为 **回文字符串** 。



##### 题目分析

第一步仍然相同，先计算并统计所有的回文子串。

第二步的重点则在于，找到这个字符串能否被分成三个回文串，即 **找到两个分割点**。完成前两个分割回文串的题目后，这个题目的思路就非常地清晰了。

可以看到，前半部分的代码都是完全重复的。

##### 代码

`p1` 和`p2` 即为分割点，字符串被分成 `S[0, p1]` , `S[p1+1, p2]` , `S[p2+1, n-1]` 三部分。注意由于要求各部分非空，要注意循环范围。

```java
public boolean checkPartitioning(String s) {

    int n = s.length();
    boolean[][] dp =  new boolean[n][n];
    for(int i=0; i<n; i++){
        dp[i][i] = true;
    }
    char[] strArray = s.toCharArray();
    for(int left=n-1; left>=0; left--){
        for(int right=left+1; right<n; right++){
            if(right-left==1){
                dp[left][right] = (strArray[left]==strArray[right]);
            }
            else
                dp[left][right] = dp[left+1][right-1]&&(strArray[left]==strArray[right]);
        }
    }
    for(int p1=0; p1<n-2; p1++){
        for(int p2=p1+1; p2<n-1; p2++){
            if(dp[0][p1]&&dp[p1+1][p2]&&dp[p2+1][n-1]){
                return true;
            }
        }
    }
    return false;
}
```

