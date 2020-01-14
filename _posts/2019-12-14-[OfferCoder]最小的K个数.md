---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]最小的K个数"               # 标题  
subtitle:   "STL，multiset使用greater<int>重载函数"  #副标题 
date:       2019-12-13 10:40:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
---

## 题目描述
> 输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4,。




## 解题思路


- 使用set集合，multiset可以允许重复元素的出现
- 使用函数模板 greater<T>模板，从大到小排序
- 首先判断的时候，如果此时元素的个数小于k的话，就直接插入了**（注意边界问题，判断的时候 一定是小于k的，没有等于k，如果有等于k，再插入一个，元素的个数就是k—+1了）**
- 使用迭代器，第一个元素就是最大的，然后判断是否比第一个元素要大，如果要大的话，删除第一个元素，然后插入小的元素就行

```C++
class Solution {
public:
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
        //vector<int> res;
        if(input.size() == 0 || k <= 0 || k > input.size()){
            return vector<int>();
        }
        multiset<int, greater<int> > res;  // 自动排序，按从大到小进行排序，第一个元素即为最大
        for(int i = 0; i < input.size(); i++){
            if(res.size() < k){            // 这个一定要注意，边界，一定是小于k的，这样插入后元素才等于k啊。。。
                res.insert(input[i]);
            }else{
                // 使用迭代器
                multiset<int, greater<int> >::iterator iter = res.begin();
                if(*iter > input[i]){
                    res.erase(*iter);
                    res.insert(input[i]);
                }
                
            }
            
        }
        return vector<int>(res.begin(), res.end());
    }
    
};
```
## 0114解题思路
- 对STL的使用不够熟悉，想不出来multiset的使用
- 对于STL，删除元素的时候，需要使用迭代器