---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]判断二叉搜索树的后序遍历"               # 标题  
subtitle:   "二叉搜索树"  #副标题 
date:       2019-12-18 11:15:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
---

## 题目描述

> 输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历的结果。如果是则输出Yes,否则输出No。假设输入的数组的任意两个数字都互不相同。

## 解题思路
- 二叉搜索树最后一个节点即为根节点
- 根节点的左节点都要比根节点要小
- 右节点都要比大节点大
- 利用这个性质，可以利用递归的思想，非别判断数组序列是否满足左右搜索子树的性质
- 注意结束的条件

```C++
class Solution {
public:
    bool VerifySquenceOfBST(vector<int> sequence) {
        if(sequence.size() <= 0){
            return false;
        }
        int root = sequence[sequence.size() - 1];
        int index = 0;
        vector<int> leftSequence;
        vector<int> rightSequence;
        int i;
        for(i = 0; i < sequence.size() - 1; i++){
            if(sequence[i] < root){
                index++;
                leftSequence.push_back(sequence[i]);
            }else{
                break;
            }
        }
        for(int j = i; j < sequence.size() - 1; j++){
            if(sequence[j] > root){
                rightSequence.push_back(sequence[j]);
            }else{
                return false;
            }
        }
        
        bool left = true, right = true;
        if(!leftSequence.empty()){
            left = VerifySquenceOfBST(leftSequence);
        }
        if(!rightSequence.empty()){
            right = VerifySquenceOfBST(rightSequence);
        }

        return left && right;
    }
};
```

## 第二遍解题思路

- 借助一个辅助函数，把当前的序列的begin，end传过去
- 递归判断左右序列是否满足对应的条件
- 序列跟根元素比较，（小于根元素的都在左边，大于根元素的都在树的右边），先判断一个pos，然后根据规则， 判断右边的data是否都比根节点小（如果不满足，直接返回false就行）
- **注意左右序列递归结束的条件为，begin > end, 递归一定要有结束条件，不然一直递归下去，会报内存使用异常，栈溢出等等**
- 

```C++
class Solution {
public:
    bool VerifySquenceOfBST(vector<int> sequence) {
        if(sequence.size() == 0){
            return false;
        }
        int begin = 0;
        int end = sequence.size() - 1;
        return isBST(sequence, begin, end);
    }
    bool isBST(vector<int> sequence, int begin, int end){
        // 递归结束条件 这个一定要有 不然会有内存超过使用限制情况
        if(begin > end){
            return true;
        }
        
        int pos = 0;
        int root = sequence[end];
        while(sequence[pos] < root){
            pos++;
        }
        for(int i = pos; i < end; i++){
            if(sequence[i] < sequence[end]){
                return false;
            }
        }
        if(isBST(sequence, begin, pos - 1) && isBST(sequence, begin + pos, end - 1)){
            return true;
        }else{
            return false;
        }
    }
};
```

  