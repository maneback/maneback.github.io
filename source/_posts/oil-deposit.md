---
title: 大庆油田
date: 2018-04-13 11:12:57
tags:
- 递归
- BFS&DFS
categories:
- Algorithm
---

**题目描述：**

在给定的n*m图中，有几个`@` 的块。块符合以下条件：其中任意的`@`均互相连接或者间接连通，两个`@` 直接相邻或者对角相邻均视为连通。

<!--more-->

我们可以这样解决这个问题，对整个图设置一个标志位，且该标记仅仅对地图上为@的点有效。按照顺序依次遍历地图上所以的位置。

若便利到@且该点未被标记，则所以与其直接相邻或者间接相邻的@点与其组合成一个块。

```cpp
#include <iostream>

using namespace std;

char maze[101][101];
bool mark[101][101];

int n, m;

int go[][2] = {
    0, -1, 0, 1, 1, 0, -1, 0, 1, 1, 1, -1, -1, 1, -1, -1
};

void DFS(int x, int y){
    for(int i = 0; i < 8; i++){
        int nx = x + go[i][0];
        int ny = y + go[i][1];
        
        if(nx<1 || nx>n || ny<1 || ny>m) coutinue;
        
        if(maze[nx][ny] == '*') continue;
        ark[nx][ny] = true;
        DFS(nx, ny);
    }
    return;
}

int main()
{
    while(cin>>n>>m){
        if(m == 0 && n == 0) break;
        
        for(int i = 1; i <= n; i++){
            for(int j = 1; j <= m; j++){
                cin>>maze[i][j];
            }
        }
        
        for(int i = 1; i <= n; i++){
            for(int j = 1; j <= m; j++){
                mark[i][j] = false;
            }
        }
        
        int ans = 0;
        for(int i = 1; i <= n; i++){
            for(int j = 1; j <= m; j++){
                if(mark[i][j] == true) continue;
                if(maze[i][j] == '*') continue;
                DFS(i, j);
                ans++;
            }
        }
        cout<<ans;
    }
    return 0;
}

```



