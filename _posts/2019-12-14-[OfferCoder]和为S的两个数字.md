---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]和为S的两个数字"               # 标题  
subtitle:   "快慢指针的应用"  #副标题 
date:       2019-12-14 20:06:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
---

## 题目描述
> 输入一个递增排序的数组和一个数字S，在数组中查找两个数，使得他们的和正好是S，如果有多对数字的和等于S，输出两个数的乘积最小的。

## 输出描述:
> 对应每个测试案例，输出两个数，小的先输出。



## 解题思路


- 快慢指针的使用
- 慢指针指向开头，快指针指向最后
- 如果结果大于指定的，快指针往前移
- 如果结果小于指定的，慢指针往后移
- 记得要有break条件，不然报程序超时


```C++
class Solution {
public:
    vector<int> FindNumbersWithSum(vector<int> array,int sum) {
        int begin = 0;
        vector<int> res;
        if(array.size() == 0){
            return res;
        }
        int end = array.size() - 1;
        // vector<int> res;
        while(begin < array.size() / 2){
            if(array[begin] + array[end] == sum){
                res.push_back(array[begin]);
                res.push_back(array[end]);
                break;
            }else if(array[begin] + array[end] < sum){
                begin++;
            }else{
                end--;
            }
        }
        return res;
    }
};
```
