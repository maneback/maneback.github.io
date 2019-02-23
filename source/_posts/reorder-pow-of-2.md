---
title: Leetcode -- Reordered pow of 2
date: 2018-08-10 09:54:41
tags:
- Leetcode
categories: Algorithm

---

原题链接：[https://leetcode.com/problems/reordered-power-of-2/description/](https://leetcode.com/problems/reordered-power-of-2/description/)

<!--more-->

## 题目大意：

任意给要一个正整数n，可以随意调换每个数字的位置，看能不能得到2的幂。

n的范围`1<n<10^9`

## 解题思路

任意调换位置都可以，那也就是说明，n的每位数字所处的位置的权重都是不重要的，只要把n逐位分解，得到的1~9每个数字的个数与某个2的幂相同的话，那就可以通过调换位置得到该2的幂。

比如2的10次幂1024，它是由1个数字`1` , 1个数字`0`， 1个数字`2`， 和1个数字`4` 组成的，只要n包含这四个数字各1个，就肯定能得到1024。也就是说我们要计数每个数字的个数。

由于1<n<10^9, 那么的话，n中最多有9个数字相同。我们可以直接用一个十位整数，每一位对应表示0~9的个数，若n与2的次幂转换之后的数字相同，则能够转换。

同样，由于n的取值范围，2的次幂最多有32个，可以枚举计算。

## 解题代码

```cpp
class Solution{
public:
    bool reorderedPowerOf2(int n){
        int cc = counter(n);
        for(int i = 0; i <=32; i++){
            if(cc == counter(1<<i)){
                return true;
            }
        }
        return false;
    }
    int counter(int n){
        int res = 0;
        while(n > 0){
            res += pow(10, n%10);
            n = n/10;
        }
    }
};
```

