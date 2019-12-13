---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]栈的压入、弹出序列"               # 标题  
subtitle:   "栈"  #副标题 
date:       2019-12-13              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
---

## 题目描述
> 输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否可能为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如序列1,2,3,4,5是某栈的压入顺序，序列4,5,3,2,1是该压栈序列对应的一个弹出序列，但4,3,5,1,2就不可能是该压栈序列的弹出序列。（注意：这两个序列的长度是相等的）

## 解题思路
- 借助辅助栈
- 遍历A序列
- 如果辅助栈的头元素与pop的元素相等，执行pop操作
- 如果这个辅助栈最后元素为空，证明pop序列是一个出栈的序列，否则，不是
- 时间复杂度O(n^2)


### 20191207

```C++
class Solution {
public:
    bool IsPopOrder(vector<int> pushV,vector<int> popV) {
        stack<int> s;
        int curPosB = 0;
        if(pushV.size() == 0 || popV.size() == 0){
            return false;
        }
        for(int i = 0; i < pushV.size(); i++){
            s.push(pushV[i]);
            while(!s.empty() && s.top() == popV[curPosB]){
                s.pop();
                curPosB++;
            }
        }
        return s.empty();
    }
};
```

### 20191213

```C++
class Solution {
public:
    bool IsPopOrder(vector<int> pushV,vector<int> popV) {
        if(pushV.size() == 0 || popV.size() == 0){
            return false;
        }
        stack<int> s1;
        int j = 0;
        for(int i = 0; i < pushV.size(); i++){
            s1.push(pushV[i]);
            while(j < popV.size() && s1.top() == popV[j]){
                s1.pop();
                j++;
            }
        }
        return s1.empty();
    }
};
```

  