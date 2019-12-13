---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]斐波那契数列"               # 标题  
subtitle:   "斐波那契"  #副标题 
date:       2019-12-13 9:16:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
---

## 题目描述
> 大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项（从0开始，第0项为0）。n<=39




## 解题思路


- 按照f(n) = f(n - 1) + f(n - 2)

```C++
class Solution {
public:
    int Fibonacci(int n) {
        if(n < 0){
            return -1;
        }
        if(n == 0){
            return 0;
        }
        int res[n + 1];
        memset(res, 0, sizeof(int) * (n + 1));
        res[1] = 1;
        res[2] = 1;
        for(int i = 3; i <= n; i++){
            res[i] = res[i - 1] + res[i - 2];
        }
        return res[n];
    }
};
```



## 题目描述
> 我们可以用2*1的小矩形横着或者竖着去覆盖更大的矩形。请问用n个2*1的小矩形无重叠地覆盖一个2*n的大矩形，总共有多少种方法？




## 解题思路

- 实质上是斐波那契数列的变形
- 想一想f(1) = 1, f(2) = 2, f3 = 3
- 按照f(n) = f(n - 1) + f(n - 2)

```C++
class Solution {
public:
    int rectCover(int number) {
        if(number< 0){
            return -1;
        }
        if(number== 0){
            return 0;
        }
        int res[number+ 1];
        memset(res, 0, sizeof(int) * (number+ 1));
        res[1] = 1;
        res[2] = 2;
        for(int i = 3; i <= number; i++){
            res[i] = res[i - 1] + res[i - 2];
        }
        return res[number];
    }
};
```

  