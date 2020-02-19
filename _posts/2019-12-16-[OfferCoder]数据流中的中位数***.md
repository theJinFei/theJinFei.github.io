---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]数据流中的中位数"               # 标题  
subtitle:   "C++最大最小堆模拟"  #副标题 
date:       2019-12-15 21:58:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
    - STL使用
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
- 主要利用STL中的，pop_heap,push_heap来维持大顶堆，小顶堆的序列**(需要掌握牢固的STL知识)**

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

## 第二遍解题思路

- 思路基本上正确
- 实现上，要记住STL的使用规则
- push_heap()是在堆的基础上进行数据的插入操作，参数与make_heap()相同，需要注意的是，只有make_heap（）和push_heap（）同为大顶堆或小顶堆，才能插入。
- pop_heap()是在堆的基础上，弹出堆顶元素。这里需要注意的是，**pop_heap()并没有删除元素，而是将堆顶元素和数组最后一个元素进行了替换，如果要删除这个元素，还需要对数组进行pop_back()操作。**
- 第三个参数是可选的，可以用伪函数less()和greater()来生成大顶堆和小顶堆，其中type为元素类型。如果只传入前两个参数，默认是生成大顶堆。

```C++
class Solution {
public:
    void Insert(int num)
    {
        if((min.size() + max.size() & 1) == 0){     // 偶数个元素 
            // 将新元素插入到最大堆里 在此之前 要比较min的最大值与num，如果最大堆 max[0] > num, 则将max最大值弹出，插入到最小堆min中
            if(max.size() > 0 && num < max[0]){
                max.push_back(num);
                // push_heap()是在堆的基础上进行数据的插入操作，参数与make_heap()相同，
                // 需要注意的是，只有make_heap（）和push_heap（）同为大顶堆或小顶堆，才能插入。
                push_heap(max.begin(), max.end(), less<int>());     // 这样能保证 max 从大到小降序，max[0]即为最大
                num = max[0];
                // 弹出首元素 pop_heap()是在堆的基础上，弹出堆顶元素。
                // 这里需要注意的是，pop_heap()并没有删除元素，而是将堆顶元素和数组最后一个元素进行了替换， -> 这样便于后面pop_back操作
                // 如果要删除这个元素，还需要对数组进行pop_back()操作。
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
        if(min.size() + max.size() == 0){
            return 0.0;
        }
        if((min.size() + max.size() & 1) == 0){
            return (min[0] + max[0]) / 2.0;
        }else{
            return (double)min[0];
        }
    }

    
private:
    // 保证最小堆里的元素每个都比最大堆的元素 要大
    // 假设每次插入的时候，如果是奇数元素，则往最大堆里插入
    // 如果此时是偶数个元素，则往最小堆里插入即可
    // 这样求中位数的话，如果此时奇数个元素的话，那么一定是最小堆的一个首元素
    // 如果是偶数个元素，最大堆的首元素+最小堆的首元素 加起来除以2就是中位数
    vector<int> min;        // 最小堆 
    vector<int> max;        // 最大堆
};
```