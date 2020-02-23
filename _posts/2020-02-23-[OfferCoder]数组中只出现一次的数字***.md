---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]数组中只出现一次的数字"               # 标题  
subtitle:   "异或，反码，补码使用"  #副标题 
date:       2020-02-23 22:34:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
    - 异或
---

## 题目描述
> 一个整型数组里除了两个数字之外，其他的数字都出现了两次。请写程序找出这两个只出现一次的数字。

## 解题思路
- 全部异或得一个结果
- 根据这个结果得到末尾1的位置
- 按照这个位置进行分组
- 每组分别进行异或 即可得最后结果

```C++
class Solution {
public:
    /*
    返回num的最低位的1，其他各位都为0
    */
    int FindFirstBit1(int num)
    {
        //二者与后得到的数，将num最右边的1保留下来，其他位的全部置为了0
        return num & (-num);
    }
    
    /*
    判断data中特定的位是否为1，
    这里的要判断的特定的位由res确定，
    res中只有一位为1，其他位均为0，由FindFirstBit1函数返回，
    而data中要判断的位便是res中这唯一的1所在的位
    */
    bool IsBit1(int data,int res)
    {
        return ((data&res)==0) ? false:true;
    }
 
    void FindNumsAppearOnce(vector<int> data,int *num1,int *num2)
    {
        if(data.size() <= 0){
            return;
        }
    
        int i;
        int AllXOR = 0;
        //全部异或
        for(i=0;i<data.size();i++)
            AllXOR ^= data[i];
    
        int res = FindFirstBit1(AllXOR);
    
        *num1 = *num2 = 0;
        for(i=0; i<data.size(); i++)
        {
            if(IsBit1(data[i], res))
                *num1 ^= data[i];
            else
                *num2 ^= data[i];
        }
    }

};
```
