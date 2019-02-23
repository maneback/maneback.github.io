---
title: 一些小想法
date: 2018-07-13 11:14:33
categories: Algorithm
description: 随时整理一些很巧妙的小方法。
---



#### 1. 如何将一个0、1组成的字符串转换成二进制数；

如果能够确定仅是由0和1组成，可以使用下面的方法：

```cpp
//这个函数是模拟了二进制数字从右到左的形成过程，很骚。
int str2bin(string str){
	int len = string.length();
    int sum = 0;
    for(int i = 0; i < len; i++){
        if(str[i] == 1) 
            sum+=1;
        sum<<=1;
    }
    sum>>=1;
    return sum；
}
```

#### 2. C++中定义空串

```cpp
string s1 = NULL;//这个没有分配空间
string s2 = ""; //这个分配了空间
```

#### 3. 定义一个m*n的二维vector

```cpp
vector<int> nums(m)//指定长度
vector<vector<int> > nums(m, vector<int>(n));//指定长度
```

