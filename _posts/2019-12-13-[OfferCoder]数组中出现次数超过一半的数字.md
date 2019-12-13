---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]数组中出现次数超过一半的数字"               # 标题  
subtitle:   "STL，mmap使用  "  #副标题 
date:       2019-12-13 10:40:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
---

## 题目描述
> 数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。例如输入一个长度为9的数组{1,2,3,2,2,2,5,4,2}。由于数字2在数组中出现了5次，超过数组长度的一半，因此输出2。如果不存在则输出0。




## 解题思路


- 可以使用键值对映射，key为对应的numbers里的元素，value则为这个元素出现的次数
- 其实这道题判断的更多的是，存不存在，如果题目说了一定存在的话，则直接排序，中间的数字即为出现次数一半的数字

```C++
class Solution {
public:
    int MoreThanHalfNum_Solution(vector<int> numbers) {
        if(numbers.size() == 0){
            return 0;
        }
        map<int, int> mmap;
        for(int i = 0; i < numbers.size(); i++){
            mmap[numbers[i]]++;
            if(mmap[numbers[i]] > numbers.size() / 2){
                return numbers[i];
            }
        }
        return 0;
    }
};
```
