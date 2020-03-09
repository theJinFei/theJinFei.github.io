---
layout:     post                    # 使用的布局（不需要改） 
title:      "[C++基础]实现lower_bound"               # 标题  
subtitle:   "STL"  #副标题 
date:       2020-03-09 16:53:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - C++基础
    - STL
---

## 实现lower_bound  
> lower_bound算法返回**第一个大于等于给定值所在的位置**。
> 设置两个指针start和last，其中start指向数组的起始位置，last指向数组末尾位置之后的位置。当start和last指向相同位置时循环结束。mid指向[start,last)区间的中间位置，当中间位置元素值大于等于给定val时，说明第一个大于等于val值在mid位置的左边，更新last为mid。当中间位置元素值小于给定的val时，说明第一个大于等于val值在mid右边，不包括mid所在的位置。更新start为mid+1。

```C++
int myLowerBound(vector<int> &data, int k)
{
	int start = 0;
	int last = data.size();
	while (start < last)
	{
		int mid = (start + last) / 2;
		if (data[mid] >= k)
		{
 			last = mid;
 		}
 		else
 		{
 			start = mid + 1;
 		}
 	}
 	return start;
}
```

## 实现upper_bound  
> upper_bound算法返回第一个大于给定元素值所在的位置
> 设置两个指针start和last，其中start指向数组的起始位置，last指向数组末尾位置之后的位置，当start和last指向相同位置时循环结束，mid指向[start,last)区间的中间位置，当中间位置元素值小于等于给定val时，说明第一个大于val值在mid位置的右边更新start为mid+1。当中间位置元素值大于给定元素时，说明第一大于在mid左边，包括mid所在位置，所以更新last为mid。

```C++
int myUpperBound(vector<int> &data, int k)
{
	int start = 0;
	int last = data.size();
	while (start < last)
	{
		int mid = (start + last) / 2;
		if (data[mid] <= k)
		{
 			start = mid + 1;
 		}
 		else
 		{
 			last = mid;
 		}
 	}
 	return start;
}
```