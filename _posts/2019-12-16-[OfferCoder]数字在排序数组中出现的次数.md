---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]数字在排序数组中出现的次数"               # 标题  
subtitle:   "二分查找"  #副标题 
date:       2019-12-16 11:00:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
---

## 题目描述
> 统计一个数字在排序数组中出现的次数。


## 解题思路

- 考验二分查找的细节
- 首先第一种解码 使用STL中的map<int, int>数据结构，遍历一遍整个数组，这样做的时间复杂度为O(n)
- 第二种，二分查找，可以把时间复杂度降为O(log(n))
- 二分查找的细节，获得最后一个k的index，获得第一个k的index，然后相减+1即为出现的次数
- 注意判断，如果二分查找中，找到k了，如果是第一个k，还要继续往前查找是不是有k的存在（改变end的指针，end = mid - 1）
- 同理，后面的k，如果二分找到了，还要继续往后查找是不是有k出现（改变begin，begin=mid+1）
- 其他的与二分类似

```C++
class Solution {
public:
    int GetNumberOfK(vector<int> data ,int k) {
        if(data.size() == 0){
            return 0;
        }
        int res = 0;
        if(data.size() > 0){
            int GetFirstK = fun_getFirstK(data, data.size(), k, 0, data.size() - 1);
            int GetLastK = fun_getLastK(data, data.size(), k, 0, data.size() - 1);
            if(GetFirstK > -1 &&  GetLastK > -1){
                res = GetLastK - GetFirstK + 1;
            }
        }
        return res;
    }
    int fun_getFirstK(vector<int> data, int length, int target, int begin, int end){
        if(begin > end){
            return -1;
        }
        
        int mid = (begin + end) / 2;
        if(data[mid] == target){
            if(mid == 0 || (mid > 0 && data[mid - 1] != target)){
                return mid;
            }else{
                end = mid - 1;
            }
        }else if(data[mid] < target){
            begin = mid + 1;
        }else{
            end = mid - 1;
        }
        return fun_getFirstK(data, length, target, begin, end);
    }
    int fun_getLastK(vector<int> data, int length, int target, int begin, int end){
        if(begin > end){
            return -1;
        }
        
        int mid = (begin + end) / 2;
        if(data[mid] == target){
            if(mid == length - 1 || (mid + 1 < length && data[mid + 1] != target)){
                return mid;
            }else{
                begin = mid + 1;
            }
        }else if(data[mid] < target){
            begin = mid + 1;
        }else{
            end = mid - 1;
        }
        return fun_getLastK(data, length, target, begin, end);
    }
    
};
```
