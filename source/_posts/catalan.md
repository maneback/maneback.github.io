---
title: 卡塔兰数
date: 2018-07-14 10:12:33
tags: 
- math
- Leetcode
categories: knowledge
mathjax: true
---

卡塔兰数是一个神奇的数字

<!--more-->

#### 1.卡塔兰数的定义

一般项公式：$C_{n} = \frac{1}{n+1}*\binom{2n}{n}$

卡塔兰数的另一种表现形式为： $C_{n} = \binom{2n}{n}- \binom{2n}{n+1}$ ，所以C~n~ 是一个自然数。

递推公式：

$C_{0} = 1, C_{n+1} = \sum_{i=0}^{n}C_{i}C_{n-i}$ 

$C_{0}=1, C_{n+1} = \frac{2(2n+1)}{n+2} C_{n}$

**性质：** 所有的奇卡塔兰数$C_{n}$都满足$n=2^{k} -1$， 其他的卡塔兰数都是偶数。

#### 2.卡塔兰数的应用：

组合数学中有非常多的组合结构可以用卡塔兰数来计数。

- $C~n~$  表示长度为2n的dyck word的个数。 dyck word是一个有n个X和n个Y 构成的字符串，且所有的前缀子串皆满足X的个数大于Y的个数。
- 将上例中的X换成左括号，Y换成右括号，表示所有包含n组括号的合法运算式的个数
- $C~n~$  表示有n个节点组成不同的二叉树的方案个数。
- $C~n~$  表示有2n+1个节点组成不同构满二叉树的方案数。
- $C~n~$  表示对{1,2…n}依序进出栈的置换个数。



Leetcode题目链接：[Leetcode 96](https://leetcode.com/problems/unique-binary-search-trees/description/)

