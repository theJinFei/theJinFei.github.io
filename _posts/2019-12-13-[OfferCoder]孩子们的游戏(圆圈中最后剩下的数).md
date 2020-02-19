---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]孩子们的游戏(圆圈中最后剩下的数)"               # 标题  
subtitle:   "环形链表"  #副标题 
date:       2019-12-13 10:17:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
    - 数据结构
    - 链表
---

## 题目描述
> 每年六一儿童节,牛客都会准备一些小礼物去看望孤儿院的小朋友,今年亦是如此。HF作为牛客的资深元老,自然也准备了一些小游戏。其中,有个游戏是这样的:首先,让小朋友们围成一个大圈。然后,他随机指定一个数m,让编号为0的小朋友开始报数。每次喊到m-1的那个小朋友要出列唱首歌,然后可以在礼品箱中任意的挑选礼物,并且不再回到圈中,从他的下一个小朋友开始,继续0...m-1报数....这样下去....直到剩下最后一个小朋友,可以不用表演,并且拿到牛客名贵的“名侦探柯南”典藏版(名额有限哦!!^_^)。请你试着想下,哪个小朋友会得到这份礼品呢？(注：小朋友的编号是从0到n-1)
如果没有小朋友，请返回-1




## 解题思路


- 写一个环形列表
- 数到m时，m出局即可
- 可以用STL的list进行模拟列表
- **vecotr是不行的**

```C++
class Solution {
public:
    int LastRemaining_Solution(int n, int m)
    {
        if(n < 1 || m < 1){
            return -1;
        }
        list<int> number;
        
        for(int i = 0; i < n; i++){
            number.push_back(i);
        }
        list<int>::iterator iter = number.begin();
        while(number.size() > 1){
            for(int i = 0; i < m - 1; i++){
                iter++;
                if(iter == number.end()){
                    iter = number.begin();
                }
            }
            list<int>::iterator iter_next = ++iter;
            if(iter_next == number.end()){
                iter_next = number.begin();
            }
            iter--;
            number.erase(iter);
            iter = iter_next;
        }
        return *iter;
    }
};
```
