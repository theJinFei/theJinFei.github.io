---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]包含min函数的栈"               # 标题  
subtitle:   "栈操作"  #副标题 
date:       2019-12-13 19:36:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
---

## 题目描述
> 定义栈的数据结构，请在该类型中实现一个能够得到栈中所含最小元素的min函数（时间复杂度应为O（1））。



## 解题思路


- 使用辅助栈
- push元素时，首先插入S1，然后判断插入的元素是否比S2栈要小于等于，如果是的话，就还要入S2栈
- pop的时候，先pop栈S1,然后判断pop的元素是否是S2栈的top元素，如果是的话，还要从S2栈里进行pop操作

### 常规版本

```C++
class Solution {
public:
    void push(int value) {
        s1.push(value);
        if(s2.empty() == true || (!s2.empty() && value <= s2.top())){
            s2.push(value);
        }
        
    }
    void pop() {
        int temp = -1;
        if(!s1.empty()){
            temp = s1.top();
            s1.pop();
        }
        if(temp == s2.top()){
            s2.pop();
        }
    }
    int top() {
        if(!s1.empty()){
            return s1.top();
        }
    }
    int min() {
        return s2.top();
    }
private:
    stack<int> s1;
    stack<int> s2;
};
```