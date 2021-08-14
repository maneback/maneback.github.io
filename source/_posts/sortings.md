---
title: 排序算法汇总
date: 2021-07-22 10:01:30
tags:
  - 排序算法
categories: Algorithm
---

对比总结常见的排序算法：

选择排序、插入排序、冒泡排序、希尔排序、归并排序、快速排序、计数排序、堆排序、基数排序

<!--more-->

#### 排序算法

均默认为升序排列。

##### 选择排序

每次遍历数组，找到未排序中的最小元素，然后放到已排序的末尾位置，直到所有的元素均排序完毕。

```python
def selection_sort(nums):
    n = len(nums)
    for i in range(n):
        for j in range(i, n):
            if nums[i]>nums[j]:
                nums[i], nums[j] = nums[j], nums[i]
        print(nums)

    return nums
```

##### 冒泡排序

每次比较两个相邻元素，较大的元素每次移动一个位置，不断地冒泡到数组末尾。

```python
def bubble_sort(nums):
    n = len(nums)
    for i in range(n):
        flag = False
        for j in range(1, n-i):
            if nums[j-1]>nums[j]:
                flag = True
                nums[j], nums[j-1] = nums[j-1], nums[j]
        if not flag:
            break
    return nums
```

##### 插入排序

每次选择一个数，在前面已排序好的序列中寻找该数字应该所在的位置，并向后移动部分已排序数字，将新数字插入到相应位置。

```python
def insert_sort(nums):
    n = len(nums)
    for c in range(1, n):
        idx = c
        n = nums[c]
        while idx>0 and nums[idx-1]>n:
            nums[idx] = nums[idx-1]
            idx-=1
        nums[idx] = n
        print(nums)
    return nums
```

##### 希尔排序

希尔排序是对插入排序的改进版。在插入排序中， 数字每一次只能移动一个位置，而希尔排序是增加移动间隔，使较小的数字能更加快速地移动到队列头部。

具体算法描述如下：

- 选择一个增量序列 $g_1 > g_2>\cdots> g_k$ 且 $g_k=1$。

- 对于每一个增量$g$, 进行一次排序。
- 在每次排序中，按照间隔进行一次插入排序。

```python
def shell_sort(nums):
    n = len(nums)
    gap=n//2
    while gap:
        for c in range(gap, n):
            i = c
            while i-gap>=0 and nums[i-gap]>gap:
                nums[i-gap], nums[i] = nums[i], nums[i-gap]
                print(nums)

                i-=gap
        gap = gap//2
    return nums
```

##### 归并排序

归并排序是一种分治思想，先将数组分为子序列，缩小问题规模，让每个子序列各自有序，然后再将有序的子序列合并成一个完整数组。这样每一次的问题规模都变为原问题的 1/2。

```python
def merge_sort(nums):
    def merge(left, right):
        res = []
        l, r = 0, 0
        while l<len(left) and r<len(right):
            if left[l]<right[r]:
                res.append(left[l])
                l+=1
            else:
                res.append(right[r])
                r+=1
        res += left[l:]
        res += right[r:]
        print( res)
        return res


    n = len(nums)
    if n==1:
        return nums
    mid = n//2
    left = merge_sort(nums[:mid])
    right = merge_sort(nums[mid:])
    
    return merge(left, right)
```

##### 快排

快排是选取一个哨兵位置，将小于哨兵的数据全部放到左边，将大于哨兵的数据全放到右边，然后再用相同的思想对左右两边继续排序。

```python
def quick_sort(nums):
    n = len(nums)
    def quick(left, right):
        # print(left, right)
        if left>=right:
            return nums
        pivot = left
        i, j = left, right
        while i<j:
            while i<j and nums[j]>nums[pivot]:
                j-=1
            while i<j and nums[i]<=nums[pivot]:
                i+=1
            nums[i], nums[j] = nums[j], nums[i]
        nums[pivot], nums[j] = nums[j], nums[pivot]
        quick(left, j-1)
        quick(j+1, right)
        # print(nums)
        return nums
    return quick(0, n-1)
```

##### 计数排序

计数排序是开辟额外的空间来存储每个值的出现次数，然后再根据计数填充数组。

```python
def count_sort(nums):
    if not nums:
        return []
    n = len(nums)
    _min = min(nums)
    _max = max(nums)
    tmp_arr = [0]*(_max-_min+1)
    for num in nums:
        tmp_arr[nums-_min]+=1
    j = 0
    for i in range(n):
        while tmp_arr[j]==0:
            j+=1
        nums[i] = j+_min
        tmp_arr[j] -= 1
    return nums 

```

##### 堆排序

堆排序是使用了堆这个数据结构来进行的排序算法。把一维的数组想象成一个二叉树（堆）结构。

过程如下：

1. 建堆，从底向上调整堆，使得父亲节点比孩子节点值大，构成大顶堆；
2. 交换堆顶和最后一个元素，重新调整堆。

```python
def heap_sort(nums):
    def adjust_heap(nums, startops, endops):
        pos = startops
        childops = pos*2+1
        if childops<endops:
            rightops = childops+1
            if rightops<endops and nums[rightops]>nums[childops]:
                childops = rightops
            if nums[childops]>nums[pos]:
                nums[pos], nums[childops] = nums[childops], nums[pos]
                adjust_heap(nums, childops, endops)

    n = len(nums)
    # 从后往前即低向上
    for i in reversed(range(n//2)):
        adjust_heap(nums, i,n)
    for i in range(n-1, -1, -1):
        nums[0], nums[i] = nums[i], nums[0]
        adjust_heap(nums, 0, i)
    return nums

```

##### 基数排序

基数排序是针对数字每一位进行排序，从最低位开始排序

```python
def radix_sort(nums):
    if not nums:
        return []
    _max = max(nums)
    max_digit = len(str(_max))
    buckList = [[] for _ in range(10)]
    div, mod = 1, 10
    for i in range(max_digit):
        for num in nums:
            buckList[num%mod//div].append(num)
        div *= 10
        mod *= 10
        idx = 0
        print(buckList)
        for j in range(10):
            for item in buckList[j]:
                nums[idx] = item
                idx += 1
            buckList[j] = []
        print(nums)
    return nums
```



##### 对比

下面对排序算法做一个总结和对比。

![image-20210814104124495](https://gitee.com/MyTypora/typorapic/raw/master/img/20210814104124.png)