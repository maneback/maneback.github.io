---
title: 图论算法(三) 最小生成树kruskal算法
date: 2018-03-31 16:08:08
tags:
- 最小生成树
- 图论
categories: Algorithm
---

Kruskal算法是一种用贪心算法的思想来寻找最小生成树的算法。在图中存在相同权值的边是也有效。

<!--more-->

## 描述

1. 初始时所有的节点属于孤立的集合
2. 按照边权值递增顺序遍历**所有** 边，若遍历到的边两个顶点属于不同的集合，则且定位最小生成树上的一条边，并将两个顶点集合合并。
3. **遍历完所有边** 之后，原图上所有节点属于同一个集合则被选取的边和节点构成一个最小生成树；否则图不连通，最小生成树不存在。

#### 在kruskal算法中，图是按照边来存储的，三元组（顶点+权值）

#### 我们要遍历所有的边，因为可能不连通，那么选取n-1 的循环则一直无法退出。

```c++
#inlcude <iostream>
#include <algorithm>
#define N 108

using namespace std;
int pre[N];

int find_root(int x){
    int r = x;
    while(pre[r] != r) r = pre[r];
    int i = x, j;
    while( i!= r){
        j = pre[i];
        pre[i] = r;
        i = pre[j];
    }
    return r;
}
struct Edge{
    int a, b;
    int cost;
    bool operator < (const Edge& A) const{
        return cost<A.cost;
    }
}v[6000];
int main()
{
    int n;
    cin>>n;
    while(1){
        if(n == 0) break;
        for(int i = 1; i <=n*(n-1)/2; i++){
            cin>>v[i].a>>v[i].b>>v[i].cost;
        }
        sort(v+1; v+1+n*(n-1)/2);
        int ans = 0;
        for(int i = 1; i <= n*(n-1)/2; i++){
            int ra = find_root(v[i].a);
            int rb = find_root(v[i].b);
            if(a!= b){
                pre[ra] = rb;
                ans+=v[i].cost;
            }
        }
        cout<<ans<<endl;
        cin>>n
    }

}
```
