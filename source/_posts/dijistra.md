---
title: 图论算法(一) 最短路径dijistra算法
date: 2018-03-31 15:04:31
tags:
- 图论
- 最短路径
categories:
- Algorithm
---



dijkstra算法是典型的用来解决最短路径的算法，用来求解从起始点到其他所有点的最短路径。该算法采用贪心的思想，每次都查找与该点距离最近的点，但是不能用来解决存在负权边的图。

<!--more-->

###数据结构：
- 无向图邻接矩阵`w[][]`
- 辅助向量`near[]`
- 标记访问向量 `visited[]`

###算法原理：
1 引入辅助向量 near[], 每个分量near[i]表示当前找到的到i节点的最短路径长度，找到这些最短路径中最短的那个。
2 每次更新向量near[]，若从起点v经过中间点k到顶点w的路径长度小于near[w]，则将near[w]修改。
3 重复迭代粗过程
4 维护一个定点集合S，表示已经找到了从起点v到该点的最短路径。

###算法过程
1. 先取一个顶点v为起点，初始化`near[]`， `near[i]`的值等于起点`v`到第i个顶点的距离`w[v][i]`，如果两者之间不存在路径，则初始化为无穷大。
2. 将`visited[v]`标记为1，表示该点已经访问过。
3. 寻找与v相邻最近的点k， 其中`near[k]`为最小值
4. 将`visited[k]`标记为1 
5. 更新`near[]`，如果`near[i]+w[k][j]<near[j]`，判断是直接连接v与j比较近，还是经过上次的最短路径点k到达j比较近。

6 重复上述过程，知道找到了所有的点。

###代码实现
```cpp
#define N 20 //顶点个数
#define NOPATH 1000 //没有路径相连

int w[N][N]; //图的邻接矩阵
int dist[N];
int near[N];
int visited[N];


void dijkstar(int n, int v){
	for(int i = 0; i < N; i ++){
		near[i] = NOPATH;
		visited[i] = 0;
	}
	for(int i = 0; i < n; i++){
		near[i] = w[v][i];
	}
	//循环找到n-1个最短路
	for(int j = 1; j < n; j++){
		int minpath = near[0];
		int k = 0;
		//找到最短路径和最近点
		for(int i = 1; i < n; i++){
			if(near[i] < minpath) minpath = near[i];
			k = i;
		}
		dist[k] = minpath;
		visited[k] = 1;
		
		for(int i = 0; i < n; i++){
			if(!visited[i] && minpath + w[k][i] < near[i]){
				near[i] = minpath + w[k][i];
			}
		}
		
	}

}
```




