---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]数组中的逆序对"               # 标题  
subtitle:   "逆序对、归并排序"  #副标题 
date:       2020-02-24 13:18:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
    - 排序
---

## 题目描述
> 在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组,求出这个数组中的逆序对的总数P。并将P对1000000007取模的结果输出。 即输出P%1000000007

## 输入描述
> 题目保证输入的数组中没有的相同的数字
数据范围：
	对于%50的数据,size<=10^4 \ 
	对于%75的数据,size<=10^5 \
	对于%100的数据,size<=2*10^5

## 实例1
> input: \
	1,2,3,4,5,6,7,0
	
> output: \
	7

## 解题思路
1. 常规思路
	- 我们的第一反应是顺序扫描整个数组。每扫描到一个数组的时候，逐个比较该数字和它后面的数字的大小。如果后面的数字比它小，则这两个数字就组成了一个逆序对。假设数组中含有n个数字。由于每个数字都要和O(n)这个数字比较，因此这个算法的时间复杂度为O(n^2)。
2. 利用归并排序 
	- 数组{7,5,6,4}为例来分析统计逆序对的过程。每次扫描到一个数字的时候，我们不拿ta和后面的每一个数字作比较，否则时间复杂度就是O(n^2)，因此我们可以考虑先比较两个相邻的数字。![归并排序](https://uploadfiles.nowcoder.com/files/20180504/7491640_1525400721676_20170710223428592)
	- (a) 把长度为4的数组分解成两个长度为2的子数组；
	- (b) 把长度为2的数组分解成两个成都为1的子数组；
	- (c) 把长度为1的子数组 合并、排序并统计逆序对 ；
	- (d) 把长度为2的子数组合并、排序，并统计逆序对；
	- 在上图（a）和（b）中，我们先把数组分解成两个长度为2的子数组，再把这两个子数组分别拆成两个长度为1的子数组。接下来一边合并相邻的子数组，一边统计逆序对的数目。在第一对长度为1的子数组{7}、{5}中7大于5，因此（7,5）组成一个逆序对。同样在第二对长度为1的子数组{6}、{4}中也有逆序对（6,4）。由于我们已经统计了这两对子数组内部的逆序对，因此需要把这两对子数组 排序 如上图（c）所示， 以免在以后的统计过程中再重复统计。
接下来我们统计两个长度为2的子数组子数组之间的逆序对。合并子数组并统计逆序对的过程如下图如下图所示。 ![合并](https://uploadfiles.nowcoder.com/files/20170711/7491640_1499735690500_20170711085550783)
	- 我们先用两个指针分别指向两个子数组的末尾，并每次比较两个指针指向的数字。如果第一个子数组中的数字大于第二个数组中的数字，则构成逆序对，并且逆序对的数目等于第二个子数组中剩余数字的个数，如下图（a）和（c）所示。如果第一个数组的数字小于或等于第二个数组中的数字，则不构成逆序对，如图b所示。每一次比较的时候，我们都把较大的数字从后面往前复制到一个辅助数组中，确保 辅助数组（记为copy） 中的数字是递增排序的。在把较大的数字复制到辅助数组之后，把对应的指针向前移动一位，接下来进行下一轮比较。
	- 过程：先把数组分割成子数组，先统计出子数组内部的逆序对的数目，然后再统计出两个相邻子数组之间的逆序对的数目。在统计逆序对的过程中，还需要对数组进行排序。如果对排序算法很熟悉，我们不难发现这个过程实际上就是归并排序。
  
```C++
class Solution {
public:
    int result = 0;
     
    int InversePairs(vector<int> data) {
         
        int len = data.size();
        vector<int> temp(len);
        MergeSort(data, temp, 0, len-1);
        return result;
    }
     
    void MergeSort(vector<int>& data, vector<int>& temp, int begin, int end)
    {
        if (begin < end)
        {
            int mid = (end - begin) / 2 + begin;
            MergeSort(data, temp, begin, mid);
            MergeSort(data, temp, mid+1, end);
            MergeArray(data, temp, begin, mid, end);
        }
    }
     
    void MergeArray(vector<int>& data, vector<int>& temp, int begin, int mid, int end)
    {
        int i = begin;
        int j = mid + 1;
        int k = 0;
         
        while (i <= mid && j <= end)
        {
            if (data[i] < data[j])
            {
                temp[k++] = data[i++];
            }
            // 若左半部分当前元素大于右半部分当前元素
            // 则左半部分当前元素后面的每个值都大于它
            else
            {
                result += (mid - i + 1);
                result %=  1000000007;
                temp[k++] = data[j++];
            }
        }
         
        while (i <= mid)
        {
            temp[k++] = data[i++];
        }
         
        while (j <= end)
        {
            temp[k++] = data[j++];
        }
         
        for (i = 0; i < k; i++)
        {
            data[begin+i] = temp[i];
        }
    }
};
```


```C++
class Solution {
public:
    int InversePairs(vector<int> data) {
        if(data.size() == 0){
            return 0;
        }
        
        // 排序的辅助数组
        vector<int> copy(data);
        return InversePairsCore(data, copy, 0, data.size() - 1) % 1000000007;
    }
    
    long InversePairsCore(vector<int>& data, vector<int>& copy, int begin, int end){
        // 如果指向相同位置，则没有逆序对
        if(begin == end){
            copy[begin] = data[end];
            return 0;
        }
        
        // 求中点
        int mid = (end + begin) >> 1;
        // 使data左半段有序，并返回左半段逆序对的数目
        long leftCount = InversePairsCore(copy, data, begin, mid);
        // 使data右半段有序，并返回右半段逆序对的数目
        long rightCount = InversePairsCore(copy, data, mid + 1, end);
        
        int i = mid; // i初始化为前半段最后一个数字的下标
        int j = end; // j初始化为后半段最后一个数字的下标
        int indexCopy = end; // 辅助数组复制的数组的最后一个数字的下标
        long count = 0;
        
        while(i >= begin && j >= mid + 1){
            if(data[i] > data[j]){
                count += j - mid;
                copy[indexCopy] = data[i];
                i--;
                indexCopy--;
            }else{
                copy[indexCopy] = data[j];
                j--;
                indexCopy--;
            }
        }
        for(; i>= begin; i--){
            copy[indexCopy] = data[i];
            indexCopy--;
        }
        for(; j >= mid + 1; j--){
            copy[indexCopy] = data[j];
            indexCopy--;
        }
        return leftCount + rightCount + count;
        
       
    }
};
```

  