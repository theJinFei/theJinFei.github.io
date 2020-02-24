---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]调整数组顺序使奇数位于偶数前面"               # 标题  
subtitle:   "使用冒泡排序"  #副标题 
date:       2020-02-24 12:34:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
    - 冒泡排序
---

## 题目描述
> 输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前半部分，所有的偶数位于数组的后半部分，并保证奇数和奇数，偶数和偶数之间的相对位置不变。

## 解题思路
- 使用冒泡排序的思想
- 如果arr[i]是偶数，arr[i + 1]是奇数，那么我们就将其换位
- 利用第二重循环来做 

```C++
class Solution {
public:
    void reOrderArray(vector<int> &array) {
        if(array.size() == 0){
            return;
        }
        int size = array.size();
        for(int i = size - 2; i >= 0; i--){
            int j = i;
            while(j < size - 1 && (array[j] & 1) == 0 && (array[j + 1] & 1) != 0){  // 这里j别溢出
                swap(array[j], array[j + 1]);
                j++;
            }
        }
    }
};
```
