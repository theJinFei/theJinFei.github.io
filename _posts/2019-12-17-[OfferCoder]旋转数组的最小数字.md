---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]旋转数组的最小数字"               # 标题  
subtitle:   "二分查找"  #副标题 
date:       2019-12-17 16:25:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
---

## 题目描述
> 把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。 输入一个非递减排序的数组的一个旋转，输出旋转数组的最小元素。 例如数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小值为1。 NOTE：给出的所有元素都大于0，若数组大小为0，请返回0。

## 解题思路

- 本题考察的是二分查找
- 无法确定中间元素是属于前面还是后面的递增子数组，只能顺序查找
- 循环条件，满足旋转的性质，即（a[left] > a[right]）
- 如果中间元素>=a[left],则易知最小元素一定是在后面的，即left = mid
- 如果中间元素中间元素位于后面的递增子数组,此时最小元素位于中间元素的前面

```C++


class Solution {
public:
    int minNumberInRotateArray(vector<int> rotateArray) {
        int size = rotateArray.size();
        if(size == 0){
            return 0;
        }//if
        int left = 0,right = size - 1;
        int mid = 0;
        // rotateArray[left] >= rotateArray[right] 确保旋转
        while(rotateArray[left] >= rotateArray[right]){
            // 分界点
            if(right - left == 1){
                mid = right;
                break;
            }//if
            mid = left + (right - left) / 2;
            // rotateArray[left] rotateArray[right] rotateArray[mid]三者相等
            // 无法确定中间元素是属于前面还是后面的递增子数组
            // 只能顺序查找
            if(rotateArray[left] == rotateArray[right] && rotateArray[left] == rotateArray[mid]){
                return MinOrder(rotateArray,left,right);
            }//if
            // 中间元素位于前面的递增子数组
            // 此时最小元素位于中间元素的后面
            if(rotateArray[mid] >= rotateArray[left]){
                left = mid;
            }//if
            // 中间元素位于后面的递增子数组
            // 此时最小元素位于中间元素的前面
            if(rotateArray[mid] <= rotateArray[right])
            {
                right = mid;
            }//else
        }//while
        return rotateArray[mid];
    }
private:
    // 顺序寻找最小值
    int MinOrder(vector<int> &num,int left,int right){
        int result = num[left];
        for(int i = left + 1;i < right;++i){
            if(num[i] < result){
                result = num[i];
            }//if
        }//for
        return result;
    }
};
```
