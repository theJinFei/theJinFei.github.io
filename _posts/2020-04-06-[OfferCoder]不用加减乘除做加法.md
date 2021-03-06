---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]不用加减乘除做加法"               # 标题  
subtitle:   "二进制模拟加法"  #副标题 
date:       2020-04-06 15:46:00             # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer
    - 位运算 
---

## 题目描述

> 写一个函数，求两个整数之和，要求在函数体内不得使用 +、-、*、/ 四则运算符号。

## 解题思路
1. 位运算
    - 首先先来看下十进制是如何计算的：
    - 相加各位的值，不进位，结果是 22, （5 + 7 = 12，舍弃进位就是2, 1 + 1 = 2 没有进位就是 22）
    - 计算进位值，得到 10。
    - 然后将上述两步得到的值重复步骤 1 和 2 。直到进位置为 0，返回不进位的值即可。
2. 那么对于二进制也可以用这种方式计算：
    - 相加各位的值，不进位，15 (1111) + 17 (10001) = 11110，其实就是将不同的位保留，相同的位归0，那么这正是位运算中的异或运算的规则，所以 15 ^ 17 即可得到不进位的值。
    - 计算进位置其实就是将只保留相同的位，也就是 15 (1111) + 17 (10001) = 00001，既然是进位值，还应该左移一位，也就是 00010，这两个小操作对应的就是位运算中的 & 和 <<，即 (num1 & num2) << 1。
    - 然后将上述两步得到的值重复步骤 1 和 2 。直到进位置为 0，返回不进位的值即可。

```C++
class Solution {
public:
    int add(int a, int b) {
        int sum = 0;
        int carry = 0;
        while(b != 0){
            sum = a ^ b;
            carry = (unsigned int)(a & b) << 1; // 无符号整形
            a = sum;
            b = carry;
        }
        return a;
    }
};
```

```C++
class Solution {
public:
    int Add(int num1, int num2)
    {
        int sum = 0;
        int carry = 0;
        while(num2 != 0){
            sum = num1 ^ num2;
            carry = (num1 & num2) << 1;
            num1 = sum;
            num2 = carry;
        }
        return num1;
    }
};
```

  