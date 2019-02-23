---
title: 素数问题
date: 2018-03-31 16:28:44
tags:
categories:
- Algorithm
---

素数相关问题：判断素数、输出前几个素数、输出(1~n范围内的所有素数)

<!--more-->

# 素数

## 一、判断素数

#### 上界只到sqrt(n)， 且先将sqrt(n)计算出来赋值给变量，不要放到for循环里面，防止在循环中多次计算耗费过多时间。

#### 所有有关于素数的计算都要从i=2开始循环

```cpp
bool is_prime(int n){
    int bound = sqrt(n)+1;
    for(int i = 2; i < bound; i++){
        if (n%i == 0) return false;
    }
    return true;
    
}

```

## 二、显示前五十个素数

> 显示前五十个素数，每十个数一行

```cpp
bool is_prime(int n){
    int bound = sqrt(n)+1;
    for(int i = 2; i < n; i++){
        if(n%i == 0) return false;
    }
    return true;
}
int res[60];
int index = 0;
int start = 2;
while(true){
    if(index == 50) break;
    if(is_prime(start++)){
        res[index++] = start;
    }
}
cout<<res[];

```

## 三、 素数筛法

若将(1~n)范围内的数使用上面的方法挨个判断，则太耗费时间。因此可以采用素数筛法。

**基本思想**  

一个不是素数的整数一定存在某个比它小的素数作为它的因数。

因此从2开始，标记每个数的倍数，若到该标记k的时候，k还没有被标记，则说明它没有比它小的因数，k为素数。已知k为素数之后，继续标记k的倍数。而且nk(n<k)已经作为n的倍数被标记过了，所有直接从**k*k** 开始标记。

若到标记k时k已经被标记，则说明k不是素数，且k的倍数肯定已经被标记了，直接跳过，看k+1

```cpp
#include <iostream>
#define n 100000
int flag[n];;
int prime[n];
using namespace std;

void init(){
    //initialize the mark = false;
    int index = 0;
    for(int i = 0; i < n; i++) 
        mark[i] = false;
    for(int i = 2; i <= n; i++){
        if(mark[i] == true)//mark is true [i] is not a prime
            continue;
        //else i is a prime, storage it.
        prime[index++] = i;
        for(int j = i*i; i < n; i++){
            mark[i] = true;
        }
    }
}
```