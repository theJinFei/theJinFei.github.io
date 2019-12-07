---
layout:     post                    # 使用的布局（不需要改） 
title:      [剑指Offer]判断二叉搜索树的后序遍历               # 标题  
subtitle:   二叉搜索树  #副标题 
date:       2019-12-07              # 时间 
author:     JinFei                    # 作者 
header-img: img/post-bg-desk.jpg    #这篇文章标题背景图片 
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

  