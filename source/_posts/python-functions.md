---
title: python内置函数
date: 2020-02-10 19:20:25
tags:
  - python
categories: 
---

介绍一些python内置函数的用法

>emumerate  / join /   zip
>



<!--more-->

#### 1. emumerate()

对于一个可迭代/可遍历的对象（如列表、字符串等），可以使用`enumerate()`同时获得索引值和数值，将得到以下object

```python
(0, lis[0]), (1, lis[1]), (2, lis[2])
```

对于一个列表，既想遍历索引，又想遍历元素时，我们可以这样写：

```python
lis = ['python', 'java', 'cpp', 'html']
for index, item in enumerate(lis):
    print(index, item)
####>>>
0 python
1 java
2 cpp
3 html
```

enumerrate 还可以接收第二个参数，作为索引的起始值。

```python
lis = ['python', 'java', 'cpp', 'html']
for index, item in enumerate(lis, 1):
    print(index, item)
####
1 python
2 java
3 cpp
4 html
```

注意：第二个参数不是修改遍历的起始位置，仍然是遍历所有元素，它只会修改索引的值。

### 2. join

python 的`join()` 函数可用于将序列中的每个元素，以指定的分隔符连接成一个新的字符串。(很是方便。)

PS: 连接符可以是字符串，而不需要是单个字符。

```python
sql = ['a', 'b', 'c', 'd']

' '.join(sql)
'-'.join(sql)
','.join(sql)
'.'.join(sql)
```

### 3. zip

`zip()`函数以可迭代的对象作为参数，将对象中对应位置的元素打包成为一个个元组，再返回有这些元组组成的一个列表。

若各个迭代器的元素的个数不相同，则返回的列表长度与最短的对象相同。

```python
a = [1,2,3]
b = [4,5,6]
c = [7,8,9]

zipped = zip(a, b)
# ====>
[(1, 4), (2, 5), (3, 6)]
```

