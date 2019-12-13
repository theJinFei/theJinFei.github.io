---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]二维数组中的查找"               # 标题  
subtitle:   "有序查找"  #副标题 
date:       2019-12-12 14:49:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
---

## 题目描述
> 在一个二维数组中（每个一维数组的长度相同），每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

## 解题思路


- 从右上角开始查找
- 如果比target大的话，只能是在下一行（因为这一行的这个元素是最大的）
- 类似的，比target小的话，只能是左边一列 

```C++
class Solution {
public:
    bool Find(int target, vector<vector<int> > array) {
        if(array.size() == 0){
            return false;
        }
        int row = array.size();
        int col = array[0].size();
        int i = 0, j = col - 1;
        while(i < row && j >= 0){
              if(array[i][j] == target){                     
                  return true;                 
              }else if(array[i][j] < target){
                  i++;
              }else if(array[i][j] > target){
                  j--;
              }
        }
        return false;
    }
};
```

  