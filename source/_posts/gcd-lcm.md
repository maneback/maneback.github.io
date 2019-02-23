---
title: 最大公约数与最小公倍数
date: 2018-03-31 16:24:14
categories:
- Algorithm
description: 用一种比较快速的方法求GCD和LCM--辗转相除法
---



# 最大公约数与最小公倍数

## 最大公约数

两个整数a,b a>b>0。a和b的最大公约数就等于b，a%b的最大公约数。因此辗转相除，直到a%b==0，那么此时b的值就等于原来a，b两个数的最大公约数。

## 最小公倍数

两个正整数a、b的最小公倍数就等于a和b的乘积除以a、b的最大公约数。

(a*b/gcd(a,b))

## 算法描述

```cpp
#include <iostream>

using namespace std;

int gcd(int a, int b){
    if(b == 0) return a;
    else return gcd(b, a%b);
}
int lcm(int a, int b){
    return a*b/gcd(a,b)
}


```
