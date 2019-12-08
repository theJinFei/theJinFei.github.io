---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]数据流中的中位数"               # 标题  
subtitle:   "C++最大最小堆模拟"  #副标题 
date:       2019-12-08              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
---

## 题目描述
> 如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。我们使用Insert()方法读取数据流，使用GetMedian()方法获取当前读取数据的中位数。


## 解题思路

- 为要求的是中位数，那么这两个堆，大顶堆用来存较小的数，从大到小排列；
- 小顶堆存较大的数，从小到大的顺序排序*，显然中位数就是大顶堆的根节点与小顶堆的根节点和的平均数。
- 保证：小顶堆中的元素都大于等于大顶堆中的元素，所以每次塞值，并不是直接塞进去，而是从另一个堆中poll出一个最大（最小）的塞值
- 当数目为偶数的时候，将这个值插入大顶堆中，再将大顶堆中根节点（即最大值）插入到小顶堆中；
- 当数目为奇数的时候，将这个值插入小顶堆中，再讲小顶堆中根节点（即最小值）插入到大顶堆中；
- 取中位数的时候，如果当前个数为偶数，显然是取小顶堆和大顶堆根结点的平均值；如果当前个数为奇数，显然是取小顶堆的根节点
- 主要利用STL中的，pop_heap,push_heap来维持大顶堆，小顶堆的序列*(需要掌握牢固的STL知识)*

```C++
class Solution {
public:
    void Insert(int num)
    {
        // 最小堆里满足各个元素都比最大堆的元素都要大
        // 如果不满足，则要进行调整
        // 如果当前元素个数为偶数，则往最小堆里插
         if(((max.size() + min.size()) & 1) == 0){
             if(max.size() > 0 && max[0] > num){
                 max.push_back(num);
                 // 调整最大堆
                 push_heap(max.begin(), max.end(), less<int>());
                 num = max[0];
                 pop_heap(max.begin(), max.end(), less<int>());
                 max.pop_back();
             }
             min.push_back(num);
             push_heap(min.begin(), min.end(), greater<int>());
         }else{
             if(min.size() > 0 && num > min[0]){
                 min.push_back(num);
                 push_heap(min.begin(), min.end(), greater<int>());
                 num = min[0];
                 pop_heap(min.begin(), min.end(), greater<int>());
                 min.pop_back();
             }
             max.push_back(num);
             push_heap(max.begin(), max.end(), less<int>());
             
         }   
    }

    double GetMedian()
    { 
        int size = max.size() + min.size();
        if(size == 0){
            return 0;
        }
        // 如果数据为偶数
        if((size & 1) == 0){
            return (max[0] + min[0]) / 2.0;
        }
        // 如果数据个数为奇数，最小堆里要比最大堆里多一个元素，即中位数肯定在最小堆里
        else{
            return min[0];
        }
    }
private:
    vector<int> max;
    vector<int> min;
};
```

  