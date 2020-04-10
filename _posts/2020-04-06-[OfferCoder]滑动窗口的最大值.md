---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]滑动窗口的最大值"               # 标题  
subtitle:   "滑动窗口固定大小"  #副标题 
date:       2020-04-06 10:42:00            # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
---

## 题目描述
> 给定一个数组和滑动窗口的大小，找出所有滑动窗口里数值的最大值。例如，如果输入数组{2,3,4,2,6,2,5,1}及滑动窗口的大小3，那么一共存在6个滑动窗口，他们的最大值分别为{4,4,6,6,6,5}； 针对数组{2,3,4,2,6,2,5,1}的滑动窗口有以下6个： {[2,3,4],2,6,2,5,1}， {2,[3,4,2],6,2,5,1}， {2,3,[4,2,6],2,5,1}， {2,3,4,[2,6,2],5,1}， {2,3,4,2,[6,2,5],1}， {2,3,4,2,6,[2,5,1]}。

## 解题思路

- 固定一个vec大小，
- 如果vec元素的个数小于固定窗口的大小，则只进行插入操作，
- 否则，找vec的最大元素，进行插入即可，
- 插入完记得删除vec的头元素，让其保持始终size等于窗口的大小
- 最后一个需要单独进行判断


```C++
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        if(nums.size() == 0 || k > nums.size()){
            return {};
        } 
        multiset<int> sets;
        vector<int> res;
        int left = 0;
        for(int i = 0; i < nums.size(); i++){
            if(sets.size() < k){
                sets.insert(nums[i]);
            }else{
                res.push_back(*sets.rbegin());
                sets.erase(sets.find(nums[i - k])); // 这里不能直接删元素，需要招
                sets.insert(nums[i]);
            }
        }
        res.push_back(*sets.rbegin());
        return res;
    }
};
```

```C++
class Solution {
public:
    vector<int> maxInWindows(const vector<int>& num, unsigned int size)
    {
       vector<int> res;
       if(size <= 0 || num.size() == 0 || size > num.size()){
           return res;
       }
       for(int i = 0; i < num.size(); i++){
           if(vec.size() < size){
               vec.push_back(num[i]);
               
           }else if(vec.size() == size){
               res.push_back(findMax(vec));
               vector<int>::iterator iter = vec.begin();
               vec.erase(iter);
               vec.push_back(num[i]);
           }
           // 最后一个需要单独进行插入
           if(i == num.size() - 1){
               res.push_back(findMax(vec));
           }
       }
       return res;
    }
    int findMax(vector<int> v){
        sort(v.begin(), v.end());
        return v[v.size() - 1];
    }
private:
    vector<int> vec;
};
```

## 第二遍解题思路及异常的情况

- 有一个坑，一直报段错误，其实是异常的问题
- 给定窗口的大小size < 0 会报段错误
- 所以判断异常的时候要三个条件，
    1. 给定的数组的size为0
    2. 给定窗口的大小大于给定数组的大小
    3. 给定窗口的size小于0

```C++
class Solution {
public:
    vector<int> maxInWindows(const vector<int>& num, unsigned int size)
    {
        vector<int> res;
        if(num.size() == 0 || size > num.size() || size <= 0){
            return res;
        }
        vector<int> temp;
        for(int i = 0; i < num.size(); i++){
            if(temp.size() < size){
                temp.push_back(num[i]);     // 不用第一个刷的那么复杂，插完进行判断就行
            }   
            if(temp.size() == size){        // 如果等于size，就erase一个。
                res.push_back(findMax(temp));
                vector<int>::iterator iter = temp.begin();
                temp.erase(iter);
            }
        }
        return res;
    }
    int findMax(vector<int> v){
        sort(v.begin(), v.end());
        return v[v.size() - 1];
    }
};
```