---
title: Python中的MapReduce用法
date: 2019-07-09 20:48:14
tags:
  - python
categories: knowledge
description: Python3 中的Map与Reduce函数用法
---

这一篇主要介绍python3中的Map函数与Reduce的用法。

Map与Reduce函数不止在python中支持，在JavaScript语言中也有对应的Map与Reduce函数，二者在思想与原理上相同，只是对应的语法标准与特定的语言相关。

- Map是将一个相同的函数应用于序列或者迭代器的每一个元素上，并返回一个经过函数处理后结果的map对象。若函数包含两个参数，则应应用到两个序列上。
- Reduce是将一个函数迭代累积的方式应用到序列或迭代器的所有元素上，并返回一个唯一的结果。函数接收两个参数。

在python3中使用Reduce函数要import一个包

```python
from functools import reduce
```

例子：

```python
from functools import recude

#1 map一个参数
x = [1,2,3,4,5]
def add1(x):
    return x+1
list(map(add5, x))

#2 map两个参数
x = [1,2,3,4,5]
y = [1,2,3,4,5]
list(map(lambda x,y: x*y, x, y))

#3 reduce
def add(x, y):
    return x+y
x = [1,2,3,4,5]
reduce(add, x) # 15
```

