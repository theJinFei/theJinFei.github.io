---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]矩形覆盖"               # 标题  
subtitle:   "斐波那契数列"  #副标题 
date:       2019-12-08              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
---

## 题目描述
> 我们可以用2*1的小矩形横着或者竖着去覆盖更大的矩形。请问用n个2*1的小矩形无重叠地覆盖一个2*n的大矩形，总共有多少种方法？



## 解题思路

- 以2x8的矩形为例。示意图如下：
- ![avatar](https://www.cuijiahua.com/wp-content/uploads/2017/11/basis_10_1.jpg)
- 我们先把2x8的覆盖方法记为f(8)。用第一个1x2小矩阵覆盖大矩形的最左边时有两个选择，竖着放或者横着放。当竖着放的时候，右边还剩下2x7的区域，这种情况下的覆盖方法记为f(7)。接下来考虑横着放的情况。当1x2的小矩形横着放在左上角的时候，左下角和横着放一个1x2的小矩形，而在右边还剩下2x6的区域，这种情况下的覆盖方法记为f(6)。因此f(8)=f(7)+f(6)。此时我们可以看出，这仍然是斐波那契数列。


```C++
class Solution {
public:
    int rectCover(int number) {
        if(number < 0){
            return -1;
        }
        if(number <= 2){
            return number;
        }
        int res[number + 1];
        memset(res, 0, sizeof(int) * (number + 1));
        res[1] = 1;
        res[2] = 2;
        for(int i = 3; i <= number; i++){
            res[i] = res[i - 1] + res[i - 2];
        }
        return res[number];
    }
};
```

  