---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]和为S的连续正数序列"               # 标题  
subtitle:   "快慢指针的应用"  #副标题 
date:       2020-02-23 20:22:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
---

## 题目描述
> 小明很喜欢数学,有一天他在做数学作业时,要求计算出9~16的和,他马上就写出了正确答案是100。但是他并不满足于此,他在想究竟有多少种连续的正数序列的和为100(至少包括两个数)。没多久,他就得到另一组连续正数和为100的序列:18,19,20,21,22。现在把问题交给你,你能不能也很快的找出所有和为S的连续正数序列? Good Luck!

## 输出描述:
> 输出所有和为S的连续正数序列。序列内按照从小至大的顺序，序列间按照开始数字从小到大的顺序



## 解题思路


- 快慢指针的使用
- **首先要知道循环结束的条件，begin从开头 到 sum/2处，想一想，如果要加sum/2以后的数字，一定是要比sum大的**
- 然后如果更新了begin，end，记得更新curSum
- 比如当curSum == sum时，begin++， end++， 此时要重新计算curSum
- 当curSum < sum的时候一样，end++后，curSum更新
- 同理 curSum > sum


```C++
class Solution {
public:
    vector<vector<int> > FindContinuousSequence(int sum) {
        vector<vector<int> > res;
        int begin = 1;
        int end = 2;
        int mid = (sum + 1) / 2;
        int curSum = begin + end;
        while(begin <= mid){
            if(curSum == sum){
                vector<int> v;
                for(int i = begin; i <= end; i++){
                    v.push_back(i);
                }
                res.push_back(v);
                
                begin++;
                end = begin + 1;
                curSum = begin + end;
            }else if(curSum < sum){
                end++;
                curSum += end;
            }else{
                curSum -= begin;
                begin++;
            }
        }
        return res;
    }

};
```


```C++
class Solution {
public:
    vector<vector<int> > FindContinuousSequence(int sum) {
        vector<vector<int>> res;
        int begin = 1;
        int end = begin + 1;
        int cur = 0;
        while(begin <= sum / 2){
            int n = end - begin + 1;
            cur = n * (begin + end) / 2;
            if(cur == sum){
                vector<int> t;
                for(int i = begin; i <= end; i++){
                    t.push_back(i);
                }
                res.push_back(t);
                begin ++;
                end = begin + 1;
            }
            else if(cur < sum){
                end++;
            }else{
                begin++;
            }
        }
        return res;
    }
};
```