---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]顺时针打印矩阵"               # 标题  
subtitle:   "数组打印"  #副标题 
date:       2019-12-18 22:31:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
---

## 题目描述
> 输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字，例如，如果输入如下4 X 4矩阵： 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 则依次打印出数字1,2,3,4,8,12,16,15,14,13,9,5,6,7,11,10.

## 解题思路

- 顺时针打印就是按圈数循环打印，
- 其中圈数circle = (min(row, col) - 1) / 2 
- 一圈包含两行或者两列，在打印的时候会出现某一圈中只包含一行，
- 要判断从左向右打印和从右向左打印的时候是否会出现重复打印，同样只包含一列时，要判断从上向下打印和从下向上打印的时候是否会出现重复打印的情况
- 判断方法是(row - i - 1) != i，或者 col - 1 - i != i

```C++
class Solution {
public:
    vector<int> printMatrix(vector<vector<int> > matrix) {
        vector<int> res;
        int row = matrix.size();
        int col = matrix[0].size();
        if(row == 1 && col == 1){
            res.push_back(matrix[0][0]);
            return res;
        }
        // 计算打印的圈数 circle = (min(row, col) - 1) / 2 + 1
        int circle = (min(row, col) - 1) / 2 + 1;
        for(int i = 0; i < circle; i++){
            // 从左到右
            for(int j = i; j < col - i; j++){
                res.push_back(matrix[i][j]);
            }
            // 从上到下
            for(int j = i + 1; j < row -i; j++){
                res.push_back(matrix[j][col - i - 1]);
            }
            // 从右向左打印（需要判断是否是最后一行，如果是单行的话，是没有这个条件的）
            for(int j = col - i - 2; j >= i && (row - i - 1) != i; j--){
                res.push_back(matrix[row - i - 1][j]);
            }
            // 从下到上打印
            for(int j = row - i - 2; (j > i && col - 1 - i != i); j--){
                res.push_back(matrix[j][i]);
            }
                                        
            
        }
        return res;
    }
    
};
```


## 第二遍刷题的问题

- **圈数忘记，可以记住，(min(row, col) - 1) / 2 + 1**
- 最后从右往左打印，从下往上打印重复条件判断忘记
- 从右往左打印  row - 1 - i != i
- 从下往上打印  col - 1 - i != i

  