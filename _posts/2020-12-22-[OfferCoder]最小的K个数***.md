---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]最小的K个数"               # 标题  
subtitle:   "STL，multiset使用greater<int>重载函数,优先级队列堆"  #副标题 
date:       2020-02-22 10:40:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
    - STL使用
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


## 使用优先级队列

- priority_queue 优先队列，其底层是用堆来实现的。在优先队列中，队首元素一定是当前队列中优先级最高的那一个。
- 在优先队列中，没有 front() 函数与 back() 函数，而只能通过 top() 函数来访问队首元素（也可称为堆顶元素），也就是优先级最高的元素。
- 此处指的基本数据类型就是 int 型，double 型，char 型等可以直接使用的数据类型，优先队列对他们的优先级设置一般是数字大的优先级高，因此队首元素就是优先队列内元素最大的那个（如果是 char 型，则是字典序最大的）。

- //下面两种优先队列的定义是等价的
```C++
priority_queue<int> q;
priority_queue<int,vector<int>,less<int> >;//后面有一个空格
<br>
// 其中第二个参数( vector )，是来承载底层数据结构堆的容器，第三个参数( less )，则是一个比较类，less 表示数字大的优先级高，而 greater 表示数字小的优先级高。

// 如果想让优先队列总是把最小的元素放在队首，只需进行如下的定义：priority_queue<int,vector<int>,greater<int> >q;：
```

```C++
class Solution {
public:
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
        if(input.size() == 0 || k > input.size()){
            return {};
        }
        priority_queue<int, vector<int>, greater<int> > pq;
        for(int i = 0; i < input.size(); i++){
            pq.push(input[i]);
        }
        vector<int> res;
        for(int i = 0; i < k; i++){
            res.push_back(pq.top());
            pq.pop();
        }
        return res;
    }
};
```