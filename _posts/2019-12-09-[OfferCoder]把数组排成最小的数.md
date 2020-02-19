---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]把数组排成最小的数"               # 标题  
subtitle:   "C++ cmp函数重载"  #副标题 
date:       2019-12-09              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
    - 排序
---

## 题目描述
> 输入一个正整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。例如输入数组{3，32，321}，则打印出这三个数字能排成的最小数字为321323。


## 解题思路

- 对vector容器内的数据进行排序，按照 将a和b转为string后 
- 若 a＋b<b+a  a排在在前 的规则排序
- 如 a = 2, b = 21 因为 212 < 221 所以 排序后为 21 2 
- to_string() 可以将int 转化为string

```C++
class Solution {
public:
    static bool cmp(int a, int b){
        string A = "";
        string B = "";
        A += to_string(a);
        A += to_string(b);
        B += to_string(b);
        B += to_string(a);
        return A < B;
    }
    string PrintMinNumber(vector<int> numbers) {
        string ans = "";
        sort(numbers.begin(), numbers.end(), cmp);
        for(int i = 0; i < numbers.size(); i++){
            ans += to_string(numbers[i]);
        }
        return ans;
    }
};
```

  