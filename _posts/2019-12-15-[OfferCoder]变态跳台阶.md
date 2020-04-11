---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]变态跳台阶"               # 标题  
subtitle:   "跳台阶问题"  #副标题 
date:       2019-12-16 22:37:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
    - dp
---

## 题目描述
> 一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶总共有多少种跳法。


## 解题思路

- 动态规划 多项式的推导

> 当n = 1 时， 只有一种跳法，即1阶跳：Fib(1) = 1;
> 当n = 2 时， 有两种跳的方式，一阶跳和二阶跳：Fib(2)  = 2;
> 到这里为止，和普通跳台阶是一样的。
> 当n = 3 时，有三种跳的方式，第一次跳出一阶后，对应Fib(3-1)种跳法； 第一次跳出二阶后，对应Fib(3-2)种跳法；第一次跳出三阶后，只有这一种跳法。Fib(3) = Fib(2) + Fib(1)+ 1 = Fib(2) + Fib(1) + Fib(0) = 4;
> 当n = 4时，有四种方式：第一次跳出一阶，对应Fib(4-1)种跳法；第一次跳出二阶，对应Fib(4-2)种跳法；第一次跳出三阶，对应Fib(4-3)种跳法；第一次跳出四阶，只有这一种跳法。所以，Fib(4) = Fib(4-1) + Fib(4-2) + Fib(4-3) + 1 = Fib(4-1) + Fib(4-2) + Fib(4-3) + Fib(4-4) 种跳法。
> 当n = n 时，共有n种跳的方式，第一次跳出一阶后，后面还有Fib(n-1)中跳法； 第一次跳出二阶后，后面还有Fib(n-2)中跳法..........................第一次跳出n阶后，后面还有 Fib(n-n)中跳法。Fib(n) = Fib(n-1)+Fib(n-2)+Fib(n-3)+..........+Fib(n-n) = Fib(0)+Fib(1)+Fib(2)+.......+Fib(n-1)。
> 通过上述分析，我们就得到了通项公式：
>                 Fib(n) =  Fib(0)+Fib(1)+Fib(2)+.......+ Fib(n-2) + Fib(n-1)
> 因此，有 Fib(n-1)=Fib(0)+Fib(1)+Fib(2)+.......+Fib(n-2)
> 两式相减得：Fib(n)-Fib(n-1) = Fib(n-1)         =====》  Fib(n) = 2*Fib(n-1)     n >= 3
> 这就是我们需要的递推公式：Fib(n) = 2*Fib(n-1)     n >= 3

```C++
class Solution {
public:
    int jumpFloorII(int number) {
         if(number <= 0){
            return 0;
        }
        if(number == 1){
            return 1;
        }
        if(number == 2){
            return 2;
        }        
        int dp[number + 1];
        memset(dp, 0, sizeof(int) * (number + 1));
        dp[0] = 0;
        dp[1] = 1;
        dp[2] = 2;

        for(int i = 3; i <= number; i++){
            dp[i] = dp[i - 1] * 2;
        }
        return dp[number];
    }
};
```


## 普通跳台阶 递归+记忆方式
```C++
class Solution {
public:
    vector<int> memo;
    int helper(int n){
        if(n == 1){
            return 1;
        }
        if(n == 2){
            return 2;
        }
        if(memo[n] == -1){
            memo[n] = helper(n - 1) + helper(n - 2);
        }
        return memo[n];
    }
    int climbStairs(int n) {
        memo.resize(n + 1, -1);
        return helper(n);
    }
};
```