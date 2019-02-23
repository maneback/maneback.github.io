---
title: 并查集
date: 2018-03-31 14:21:13
tags:
- 并查集
- 图论
categories:
- Algorithm
---



并查集主要用来解决动态连通性问题。

<!--more-->

比如：随意给两个点，问是否联通，或者问图有几个联通分支，即被分为了几个块，畅通工程需要修几条路等等。
并查集有一个整数型的数组和两个函数组成。

- 数组 `pre[ ]` 记录了每个节点的前导节点是什么；

- `find()` 是查找根节点

- `join()` 是合并两个元素所在的集合


需要实现的操作有：**判断两个元素是否属于一个集合、合并两个集合**

**注意：**每次查找之后进行路径压缩，将整棵树上的所有节点的前导节点都改为这棵树的根r，提高下次查找时候的查找效率。
```cpp
	//路径压缩, 将这一集合所以的pre都改为r
	while(i != r){
		j = pre[i];
		pre[i] = r;
                i = j;
	}
```
代码如下
```cpp

#define N 1000

int pre[N];
//初始化
for(int i = 0; i < N; i++){
  pre[i] = i;
}

int find(int x){
	//找x所在树的根节点
	int r = x;
	while(r != pre[r]) 
		r = pre[r];
	int i = x, j;
	//路径压缩, 将这一集合所以的pre都改为r
	while(i != r){
		j = pre[i];
		pre[i] = r;
        i = j;
	}
	return r;
}

int join(int x, int y){
	int rx = find(x);
	int ry = find(y);
	if(rx != ry){
		pre[rx] = ry; //将x的根节点接到y上面
	}
}
```