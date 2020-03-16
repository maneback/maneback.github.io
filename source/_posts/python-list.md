---
title: python3中列表的操作
date: 2020-02-09 17:37:13
tags:
  - python
categories: knowledge
description: 介绍python中列表常用的操作
---

列表(list)是python中的一个基础的数据结构。

- 创建列表，访问、修改列表中的值

```python
list0 = []
list1 = [1,2,3,'a','b','c']
# 通过index索引值访问列表中的值，更新列表中的值
print(list1[0])
list1[0] = 'a'
```

- `+`可以用来组合连接列表，`*`用来重复列表元素，如

```python
len(list1) # result =6
[1,2,3] + [4,5,6] # result = [1,2,3,4,5,6]
['aa'] * 3 # result = ['aa','aa','aa']
3 in [1,2,3] # False
'3' in [1,2,3] # False
for x in [1,2,3]:
    print(x)
```

- 通过index截取

```python
# 包括元素list[l]，不包括元素list[r]
list[l:]
list[:r]	
list[-a]
list[l:r]
```

- 有用的函数方法

```python
len(list) #长度
max(list) #最大值
min(list) #最小值
list.append(pp) #在末尾添加新的元素
list.count(pp) #计算pp在列表中出现的次数
list.index(pp) #返回pp值第一个匹配项的索引位置
list.insert(index, pp) # 将元素pp插入到index位置
list.pop() #移除列表中的最后一个元素，并返回该元素的值
list.remove(pp) #移除列表中pp的第一个匹配项
list.reverse() # 反向列表元素
list.sort(key=None, reverse=False) # 对列表进行排序
```