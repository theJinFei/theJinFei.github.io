---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]剪绳子"               # 标题  
subtitle:   "dp"  #副标题 
date:       2020-04-06 16:20:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
    - dp
---

## 题目描述

> 给你一根长度为n的绳子，请把绳子剪成整数长的m段（m、n都是整数，n>1并且m>1），每段绳子的长度记为k[0],k[1],...,k[m]。请问k[0]xk[1]x...xk[m]可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

## 输入描述
> 输入一个数n，意义见题面。（2 <= n <= 60）
> 输入:
> 8
> 输出:
> 18

## 解题思路
- dp
- f(n)的时候，可以切一刀f[i],剩下的f[n - i]
- 初始化的时候要注意初值问题
- dp[i] = max(dp[j], dp[i - j]);

```C++
class Solution {
public:
    int cuttingRope(int n) {
        vector<int> dp(n + 1, 0);
        if(n < 2){
            return 1;
        }
        if(n == 2){
            return 1;
        }
        if(n == 3){
            return 2;
        }
        dp[0] = 0;
        dp[1] = 1;
        dp[2] = 2;
        dp[3] = 3;
        int res = 0;
        for(int i = 4; i <= n; i++){
            for(int j = 1; j <= i / 2; j++){
                dp[i] = max(dp[i], dp[j] * dp[i - j]);
            }
        }
        return dp[n];
    }
};
```

## 大数溢出的问题

- 选择最多的3

```C++
class Solution {
public:
    int cuttingRope(int n) {
        if(n<=3) return n-1;
        int mod = 1000000007;
        long long res = 1;
        while(n>4){
            res*=3;
            res%=mod;
            n-=3;
        }
        res = res*n%mod;
        return res;
    }
};
```

```C++
class Solution {
public:
    int Rope(int number) {
        if(number <= 0){
            return -1;
        }
        if(number == 1){
            return 1;
        }
        if(number == 2){
            return 1;
        }
        if(number == 3){
            return 2;
        }
        
        int dp[number + 1];
        memset(dp, 0, sizeof(int) * (number + 1));
        dp[0] = 0;
        dp[1] = 1;
        dp[2] = 2; // init the dp
        dp[3] = 3;
        int res = 0;
        for(int i = 4; i <= number; i++){
            res = 0;
            for(int j = 1; j <= i / 2; j++){
                if(dp[j] * dp[i - j] > res){
                    res = dp[j] * dp[i - j];
                }
            }
            dp[i] = res;
        }
        return dp[number];
    }
};
```

## 0229解题思路
- dp数组需要先初始化为0


  

dp[0] = 0;
dp[1] = 1;
dp[2] = 2;
dp[3] = 3；
for(int i = 4； i <= n; i++){
    for(int j = 1; j < i; j++){
        dp[i] = max(dp[i], dp[i - j] * dp[j])
    }
}
return dp[n];
