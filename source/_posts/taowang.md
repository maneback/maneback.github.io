---
title: 胜利大逃亡
date: 2018-04-13 09:34:47
tags: 
- BFS&DFS
categories: Algorithm
---

广度优先搜索例子

<!--more-->

### 题目描述：

从一个A\*B\*C的三维城堡逃出去，从`(0,0,0)` 位置逃至`(A-1, B-1, C-1)` 已知魔王将在T时刻回来，且每分钟只能走到相邻的六个坐标中的一个，现在给你城堡的地图，问能否在魔王回来之前离开城堡？

输入 A, B, C, T 后面的数据代表迷宫的布局， 0代表路， 1代表墙。

若能逃出去，则输出至少需要多少时间；否则输出`-1`。

### 题目解答：

我们用以下的结构体来保存每一个状态：

```cpp
struct N{
    int x, y, z;
    int t;
};
```

其次，为了实现各个状态按照被查找到的顺序依次转移扩展，我们需要使用一个队列。将每一扩展得到的新状态放到队列中。 

最后，为防止对无效状态的搜索，需要设置一个标记数组`mark[a][b][c]` ,当已经得达到包括坐标`(x, y, z)` 的状态之后，将`mark[x][y][z]` 标记为true， 下次再到达这个坐标之后，不再处理。

```cpp
#include <iostream>
#include <queue>

using namespace std;

bool mark[50][50][50];
int maze[50][50][50];

struct N{
  int x, y, z;
  int t;
};

queue<N> Q;
int go[][3] = {
    1, 0, 0,
    -1, 0, 0,
    0, 1, 0,
    0, -1, 0,
    0, 0, 1,
    0, 0, -1
};

int BFS(int a, int b, int c){
    while(!Q.empty()){
        N cur = Q.top();
        Q.pop();
        for(int i = 0; i < 6; i++){
            int nx = cur.x + go[i][0];
            int ny = cur.y + go[i][1];
            int nz = cur.z + go[i][2];
            if(nx>=a || nx<0 || ny>=b || ny<0 || nz>=c || nz<0) continue;
            if(maze[nx][ny][nz] == 1) continue; //qiang
            if(mark[nx][ny][nz] == true) continue;

            N.tmp;
            tmp.x = nx;
            tmp.y = ny;
            tmp.z = nz;
            tmp.t = cur.t+1;
            Q.push(tmp);
            mark[nx][ny][nz] = true;
            if(nx==a-1 && ny==b-1 && nz==c-1) return tmp.t;
        }
    }
    return -1;
}

int main()
{
    int test;
    int A, B, C, T;
    while(test--){
        cin>>A>>B>>C>>T;
        for(int i = 0; i < A; i++){
            for(int j = 0; j < B; j++){
                for(int k = 0; k < C; k++){
                    cin>> maze[i][j][k];
                }
            }
        }
        for(int i = 0; i < A; i++){
            for(int j = 0; j < B; j++){
                for(int k = 0; k < C; k++){
                    mark[i][j][k] = false;
                }
            }
        }        
        while(!Q.empty()) Q.pop(); //把上次的队列清空；
        mark[0][0][0] = true;
        N tmp;
        tmp.x = 0;
        tmp.y = 0;
        tmp.z = 0;
        tmp.t = 0;
        Q.push(tmp);
        int r = BFS(0, 0, 0);
        if(r>=0 && r<=t) return t;
        else return -1;
    }
}
```

**注：** 这个是使用队列来实现BFS 的。