---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]求第n个丑数"               # 标题  
subtitle:   "丑数"  #副标题 
date:       2020-02-29 21:43:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
---

## 题目描述

> 把只包含质因子2、3和5的数称作丑数（Ugly Number）。例如6、8都是丑数，但14不是，因为它包含质因子7。 习惯上我们把1当做是第一个丑数。求按从小到大的顺序的第N个丑数。

## 解题思路
- 2^x, 3^y, 5^z
- res[num_2] * 2, res[num_3] * 3, res[num_5] * 5 三个数取最小即为当前第i个丑数


```C++
class Solution {
public:
    int GetUglyNumber_Solution(int index) {
        if(index <= 0){
            return 0;
        }
        int res[index];
        memset(res, 0, 1);
        int num_2 = 0;
        int num_3 = 0;
        int num_5 = 0;
        res[0] = 1;
        for(int i = 1; i < index; i++){
            res[i] = getMin(res[num_2] * 2, res[num_3] * 3, res[num_5] * 5);
            if(res[i] == res[num_2] * 2){
                num_2++;
            }
            if(res[i] == res[num_3] * 3){
                num_3++;
            }
            if(res[i] == res[num_5] * 5){
                num_5++;
            }
        }
        return res[index - 1];
    }
    int getMin(int a, int b, int c){
        return min(c, min(a, b));
    }
};
```

  