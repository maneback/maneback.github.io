---
title: 图论算法(四)最小生成树Prim算法
date: 2018-03-31 16:16:24
tags:
- 最小生成树
- 图论
categories: 
- Algorithm
description: Prim 算法是从点的角度考虑构建最小生成树
---

- Prim 算法是从点的角度考虑构建最小生成树。
    大致思想是：设图G的顶点集合为U， 首先选取任意的顶点a为起始点，将该点加入到集合V中，再从集合U-V中找到另一个顶点b，使得b到V中的**任意一个**顶点的距离最短，此时，将b也加入到集合V，以此类推，知道所有的顶点都已经加入到集合V当中，就构建出了一棵最小生成树(MST)。
    　　因为有N的顶点，所以树就应该有N-1条边，每次向集合V中加入一个顶点，就意味着找到了MST的一条边。

###设置两个数据结构：
- `lowcost[i]`: 表示以i为重点的边的最小权值，当`lowcost[i]`为0，说明最小权值为0，该顶点已经加入到了MST。
- `mst[i]`: 表示对应的`lowcost[i]`的起点，即`<mst[i],i>`是MST当中的一条边，当`mst[i] = 0` 就表示起点加入MST.

**另：**图的邻接矩阵 ：w[N][N]
##样例代码：
```cpp
#define N 100
#define MAXCOST 10000

int graph[N][N]

int prim(int graph[][N], int n){
	int lowcost[N];
	int mst[N];
	//初始化lowcost 和 mst 数组
	for(int i = 2; i <= n; i++){
		lowcost[i] = graph[1][i];
		mst[i] = 1;
	}
	mst[1] = 0; // v1 in V
	int sum = 0;
	
	
	for(int i = 0; i < n-1; i++){
		int mincost = MAXCOST;
		int findp = 0;
		for(int j = 2; j <= n; j++){
			if(lowcost[j] < mincost && lowcost[j] != 0){
				mincost = lowcost[j];
				findp = j;
			}
		}
		sum += mincost;
		lowcost[findp] = 0;	
		for(int j = 2; j <= n; j++){
			if(graph[findp][j] < lowcost[j]) {
				lowcost[j] = graph[findp][j];
				mst[j] = findp;
			}
		}
		
	}
	
	return sum;
	
}

```