---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]排序算法总结"               # 标题  
subtitle:   "排序"  #副标题 
date:       2020-03-01 10:50:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
    - 排序
---

## 快速排序

```C++ 
void QuickSort(vector<int>& data, int begin, int end){
    if(begin < end){
        int pivot = parition(data, begin, end);
        QuickSort(data, begin, pivot - 1);
        QucikSort(data, pivot + 1, end);
    }
}
void parition(vector<int>& data, int begin, int end){
    int pivot = data[begin];
    while(begin < end){
        while(begin < end && data[end] > pivot){
            end--;
        }
        data[begin] = data[end];
        while(begin < end && data[begin] < pivot){
            begin++;
        }
        data[end] = data[begin];
    }
    data[begin] = pivot;
    return pivot;
}
```

## 归并排序

```C++
void MergeSort(vector<int>& data, vector<int>& temp, int begin, int end){
    if(begin < end){
        int mid = (begin + end) / 2;
        MergeSort(data, temp, begin, mid);
        MergeSort(data, temp, mid + 1, end);
        Merge(data, temp, begin, mid, end);
    }
}
void Merge(vector<int>& data, vector<int>& temp, int begin, int mid, int end){
    int i = begin;
    int j = mid + 1;
    int k = 0;
    while(i <= mid && j <= end){
        if(data[i] < data[j]){
            temp[k++] = data[i++];
        }else{
            temp[k++] = data[j++];
        }
    }
    while(i <= mid){
        temp[k++] = data[i++];
    }
    while(j <= mid){
        temp[k++] = data[j++];
    }
    k = 0;
    while(begin <= end){
        data[begin++] = temp[k++];
    }
}
```

## 堆排序
```C++
void heapSort(vector<int>& data){
    int size = data.size();
    for(int i = size / 2 - 1; i >= 0; i--){
        adjustHeap(data, i, size);
    }
    for(int i = size - 1; i > 0; i--){
        swap(data[0], data[i]);
        adjustHeap(data, 0, i);     // 从0开始 重新对堆进行排序
    }
}
// 从i的位置 往下查找 有没有比这个位置大的 如果有 就记录下来 最后替换
void adjustHeap(vector<int>& data, int i, int length){
    int temp = data[i];
    for(int k = 2 * i + 1; k < length; k = 2 * k + 1){
        if(k + 1 < length && data[k] < data[k + 1]){
            k++;
        }
        if(data[k] > temp){
            datat[i] = data[k];
            i = k;
        }else{
            break;
        }
    }
    data[i] = temp;
}
```

## 希尔排序
```C++

```
