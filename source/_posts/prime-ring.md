---
title: 素数环
date: 2018-04-13 10:45:12
tags:
- 递归
- BFS&DFS
categories:
- Algorithm

---

**题目描述：**

题目的大意是给定1到n的数字中，将数字一次填入环中，使得环中任意两个相邻的数字的和为素数。

<!--more-->

而且对于给定的n， 按照字典序由小到大输出所有符合条件的解。（同时第一个数恒定为1）。

为了解决该问题，我们采用回溯法枚举每一个值。

当第一个数为1确定时，尝试放入第二个数，使其与1的和为素数，放入后再尝试第三个数，使其与第二个数的和为素数。直到所有的数全放入换种，且最后一个与数与1的和也为素数，那么这个方案即为一个答案。

若在过程中，发现当前无论防止任何之前未放置过的数都不能满足条件，那么回溯改变上一个数，直到产生需要的答案，或者不再存在更多的解。

```cpp
#include <iostream>
#include <queue>

using namespace std;

int ans[20]; //保存环中每一个被放入的数
bool hash[20]; //标记之前已经放入环中的数
int n;
int prime[] = {2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41};

bool judge(int x){
    for(int i = 0; i < 13; i++){
        if(prime[i] == x) return true;
    }
    return false;
}

void check(){
    //ans[n]+ans[1] 不是素数，不是解。
    if(judge(ans[n]+ans[1]) == false) return;
    
    for(int i = 1; i <= n; i++){
        cout<<ans[i]<<" ";
    }
    cout<<endl;
}
//num表示已经放入环中的数字
void DFS(int num){
    if(num > 1){
        if(judge(ans[num]+ans[num-1]) == false) return;
    }
    if(num == n){
        check();
        return;
    }
    for(int i = 2; i <= n; i++){
        if(hash[i] == false){
            hash[i] = true;
            ans[num] == i;
            DFS(num+1);
            hash[i] = false; //返回后清除标记
        }
    }
} 

int main()
{
    int case = 0;
    while(cin>>n){
        case++;
        for(int i = 0; i <= n; i++) hash[i] = false;
        ans[1] = 1;
        cout<<"case "<<case<<":"<<endl;
        hash[1] = true;
        DFS(1);
        cout<<endl;
    }
    return 0;
}
```

## # 这是递归函数的第一个应用，回溯法枚举。