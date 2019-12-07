---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]矩阵中的路径"               # 标题  
subtitle:   "dfs模拟"  #副标题 
date:       2019-12-07              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
---

## 题目描述

> 请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一个格子开始，每一步可以在矩阵中向左，向右，向上，向下移动一个格子。如果一条路径经过了矩阵中的某一个格子，则该路径不能再进入该格子。 例如 a b c e s f c s a d e e 矩阵中包含一条字符串"bcced"的路径，但是矩阵中不包含"abcb"路径，因为字符串的第一个字符b占据了矩阵中的第一行第二个格子之后，路径不能再次进入该格子。

## 解题思路
- 类似于上一题的机器人路径
- dfs遍历当前节点，看是否满足条件
- 如果存在，直接返回即可
- 注意边界



```C++
class Solution {
public:
    bool hasPath(char* matrix, int rows, int cols, char* str)
    {
       bool flag[rows * cols];
       for(int i = 0; i < rows * cols; i++){
           flag[i] = false;
       }
        bool res = false;
        for(int i = 0; i < rows; i++){
            for(int j = 0; j < cols; j++){
                res = fun_helper(matrix, rows, cols, i, j, str, 0, flag);
                if(res){
                    return true;
                }
            }
        }
        return false;
    }
    bool fun_helper(char* matrix, int rows, int cols, int r, int c, char * str, int curlen, bool* flag){
        if(r < 0 || r >= rows || c < 0 || c >= cols || flag[r * cols + c] == true || matrix[r * cols + c] != str[curlen]){
            return false;
        }
        if(curlen == strlen(str) - 1){
            return true;
        }
        flag[r * cols + c] = true;
        if(fun_helper(matrix, rows, cols, r - 1, c, str, curlen+1, flag) ||
          fun_helper(matrix, rows, cols, r, c - 1, str, curlen+1, flag) ||
           fun_helper(matrix, rows, cols, r + 1, c, str, curlen+1, flag) ||
           fun_helper(matrix, rows, cols, r, c + 1, str, curlen+1, flag)){
            return true;
        }
        flag[r * cols + c] = false;
        return false;
    } 


};
```

  