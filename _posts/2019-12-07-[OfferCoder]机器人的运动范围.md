---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]机器人的运动范围"               # 标题  
subtitle:   "dfs"  #副标题 
date:       2019-12-07              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
---

## 题目描述

> 地上有一个m行和n列的方格。一个机器人从坐标0,0的格子开始移动，每一次只能向左，右，上，下四个方向移动一格，但是不能进入行坐标和列坐标的数位之和大于k的格子。 例如，当k为18时，机器人能够进入方格（35,37），因为3+5+3+7 = 18。但是，它不能进入方格（35,38），因为3+5+3+8 = 19。请问该机器人能够达到多少个格子？

## 解题思路
- dfs
- 首先利用一个全局的flag表示被没被访问过
- 终止的条件（比如说<0 || >= rows等）
- count传引用，累加即可


```C++
class Solution {
public:
    int movingCount(int threshold, int rows, int cols)
    {
        if (threshold <= 0 || rows <= 0 || cols <= 0)
            return 0;
        bool flag[rows * cols];
        for(int i = 0; i < rows * cols; i++){
            flag[i] = false;
        }
        int count = 0;
        countSteps(threshold, rows, cols, 0, 0, flag, count);

        return count;
    }
    void countSteps(int threshold, int rows, int cols, int r, int c, bool* visited, int& count){
        if(r < 0 || r >= rows || c < 0 || c >= cols || visited[r * cols + c] == true|| countNum(r) + countNum(c) > threshold){
            return;
        }
        count++;
        visited[r * cols + c] = true;
        countSteps(threshold, rows, cols, r - 1, c, visited, count);
        countSteps(threshold, rows, cols, r , c - 1, visited, count);
        countSteps(threshold, rows, cols, r + 1, c, visited, count);
        countSteps(threshold, rows, cols, r , c + 1, visited, count);
    }
    int countNum(int t){
        int res = 0;
        while(t != 0){
            res += t % 10;
            t /= 10;
        }
        return res;
    }
};
```

  