---
title: 回溯算法（一）
date: 2021-08-25 13:29:43
tags:
  - Leetcode
  - 回溯法
categories: Algorithm
description: 回溯算法

---

#### 回溯法介绍

最近在刷题的时候，发现之前手拿把攥的 回溯算法 下手的时候都经常有一点点疑惑和迟疑了。可能是太久没看它了，所以现在再来总结一下吧。

回溯算法可以理解为一个 n 叉树的查找，再加上剪枝的过程（也可能不存在剪枝）。在每一层中试探所有对可能性，走到叶子节点后，再回溯到父节点，试探下一个可能。因此如果理解了树的三种遍历过程，就能更容易地理解回溯法了。

回溯法一定是与**递归**紧密联系的，这样的话，就可以简化一下，在每一次递归中，只需要考虑当前步骤可选的操作以及如何进行到下一步，判断回溯是否结束。

回溯法的框架如下：

```python
def back(step, trace, choices, n):
    if(step==n)
        ans.append([_ for _ in trace])
        return;// 探索到根节点, n 表示最大深度
    
    // 尝试每个可行的选择，可能要对可行性进行判断。
    for ch in choices:
        traces.append(ch)//加入选择
        back(step+1, trace, choices, n)
        traces.pop() // 回溯回来，撤销选择

ans = []
nums = [1,2,3,4,5,6]
trace
back(0, trace, nums, len(nums))
    
```

#### 例题1 全排列

链接： [Leetcode-46](https://leetcode-cn.com/problems/permutations/*)

>  给定一个**不含重复数字**的数组 `nums` ，返回其 **所有可能的全排列** 。你可以 **按任意顺序** 返回答案。

##### 回溯法解题思路

把问题套在我们的回溯法上，对全排列来讲假设数组长度为 `n`，那么数组的每个位置便为 `step`。即在每一步中，是给数组中的第`step`个位置安排数字，前面被安排的 `step-1` 个数字便为`trace`。

这里不用显式地记录 `step` 可以直接用 `trace` 的长度来表示。同时还需判断当前数字，是否已经在 `trace` 中了。

##### 样例代码

```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        res = []
        def back(trace, n, nums):
            if(len(traces)==n):
                res.append([i for i in trace])
            for ch in nums:
                if ch not in trace:
                    trace.append(trace)
                    back(trace, n, nums)
                    trace.pop()
            n = len(nums)
            back([], n, nums)
            return res

```

#### 例题2 全排列II

链接 [Leetcode-47](https://leetcode-cn.com/problems/permutations-ii/)

>  给定一个可包含重复数字的序列 `nums` ，**按任意顺序** 返回所有不重复的全排列。

##### 回溯法思路

这道题与上一个题的区别在于，数组中含有重复的数组，而在排列上，相同数字又是不影响排列的。如数组`[3,3]`， 其只有一个排列 `[3,3]`，而不把它认为成是两个排列。那么我们下一步要解决的重要问题在于**如何判断重复**以及**如何去重**。

方法便是，把数组进行排序，这样的话，所有相同的数字便挨在了一起，这样的话只要判断` ch` 是否与上一个数字相同，以及上一个数字的使用状态。我们用`num` 表示排序后的数组，用 `used` 记录每个数字的使用情况。

如果 `num[i]` 与 `num[i-1]` **相等**，且 `num[i-1]` **未被使用**的话，那么便跳过当前数字 `num[i]`。这句话便是去重的关键，可能不太好理解。我们来画个树理解一下，以数组 `[1,1,2,3]` 为例。

节点里面的数组表示`used`数组，直线表示前进过程，曲线表示回溯过程。

看一下第 1 层的黄色节点。表示我们即将选择数组中的第二个 `1`。而我们怎么判断它是与前面那个数字重复的呢？只有在**前一个节点刚刚完成回溯的时候**，才会出 现 `num[i-1]` 未被使用的情况，这时候如果我们继续探索 `num[i]` 便是一个重复的过程。

也就是说这样就能保证每个重复出现的数字在**每一层**中只会被选择一次，而在不同的层，它们是没有影响的，因为`used[i-1]=1` 时，`num[i-1]`一定是在上一层中添加的，而不是在这一层。

![image-20210825141431597](https://gitee.com/MyTypora/typorapic/raw/master/img/20210825141431.png)

##### 样例代码

```python
class Solution:
    def permuteUnique(self, nums: List[int]) -> List[List[int]]:
        self.res = []
        
        def back(trace, nums, used):
            if(len(traces)==len(nums)):
                self.res.append([i for i in trace])
                return
           	for i in range(len(nums)):
                if i>0 and nums[i]==nums[i-1] and not used[i-1]:
                    continue ## 去重
                    
                    if not used[i]:
                        used[i]= True
                        trace.append(nums[i])
                        back(trace, nums, used)
                        used[i] = False
                        trace.pop()
            
        nums.sort()
       	used = [False]*len(nums)
        back([], nums, used)
        return self.res
            
```

#### 例题3 子集I

链接 [Leetcode-78](https://leetcode-cn.com/problems/subsets/)

> 给你一个整数数组 `nums` ，数组中的元素 **互不相同** 。返回该数组所有可能的子集（幂集）。
>
> 解集 **不能** 包含重复的子集。你可以按 **任意顺序** 返回解集。

子集和全排列的区别在于，全排列仅在叶子结点更新要返回的答案集合，而子集要在每个节点都要更新答案集合（每个节点都是一个子集）。

还有就是，元素添加的顺序是不重要的，因此我们每次的选择，不是从头开始选，而是从当前位置 `step` 以后开始选，保证子集中的元素是按照其在原数组中的顺序被添加到其中的。如` [1,2,3,4,5]`。当我们回溯到 `3` 的时候，后续只考虑 添加 `4,5`。而不能乱序添加，这样的话，结果中就不会出现重复子集了。

```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        self.res = []
        def back(step, trace, nums, used):
           	self.res.append(trace)
            if len(trace)>=len(nums):
                return
            for ch in range(idx, len(nums)):
                if not used[ch]:
                    used[ch] = True
                    trace.append(nums[ch])
                    back(ch+1, trace, nums, used) # 下一层只能添加，ch 之后的数字
                    used[ch] = False
                    trace.pop()
		n = len(nums)
        used = [False]*n
        back(0, [], nums, used)
        return self.res
```

#### 例题4 子集II

链接 [Leetcode-90](https://leetcode-cn.com/problems/subsets-ii/)

> 给你一个整数数组 nums ，其中可能包含重复元素，请你返回该数组所有可能的子集（幂集）。
>
> 解集 不能 包含重复的子集。返回的解集中，子集可以按 任意顺序 排列。
>

把例题2 和 例题3 结合一下，就能得这个题目的答案了。同样是**子集** + **去重**。

```python
class Solution:
    def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
        self.res = []
        def bp(trace, nums, idx):
            self.res.append([i for i in trace])
            if idx>=len(nums):
                return 

            for i in range(idx, len(nums)):
                if i>0 and nums[i]==nums[i-1] and not used[i-1]: # 去重
                    continue
                
                used[i] = True
                trace.append(nums[i])
                bp(trace, nums, i+1)  # 下一层只添加 i 之后的数字
                used[i] = False
                trace.pop()

        if not nums:
            return []
        nums.sort()
        used = [False]*len(nums)
        bp([], nums, 0)
        return self.res
```

