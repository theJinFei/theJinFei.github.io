---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]矩阵中的路径"               # 标题  
subtitle:   "dfs模拟"  #副标题 
date:       2019-12-15 10:07:00              # 时间 
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

## 第二遍解题思路

**有几个注意的点**
1. 怎么调用fun_helper这个函数，矩阵中的每个位置都有可能是从那个位置出发然后查找路径，所以第一个注意的一定是 两个for循环调用helper函数
2. 判断正确的条件，生成一个此时字符串的长度，如果这个长度等于指定目标字符串长度减1的话，就证明上面所走的路径是正确的，所以返回正确即可
3. 条件判断的时候，判断矩阵中此时的位置的元素 matrix[r * cols + c] 是否等于 str[curlen]，如果不相等，返回false即可。
4. 把当前访问过的元素置为true时，下一步应该是个if判断，判断这个元素的上下左右，能不能到达，dfs思想，只要有一条路径，就可以返回正确
5. 需要把这次访问过的路径重新置为False，然后才返回。
