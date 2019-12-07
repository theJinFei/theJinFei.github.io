---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]二叉树的下一个节点"               # 标题  
subtitle:   "二叉树的遍历"  #副标题 
date:       2019-12-07              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
---

## 题目描述

> 给定一个二叉树和其中的一个结点，请找出中序遍历顺序的下一个结点并且返回。注意，树中的结点不仅包含左右子结点，同时包含指向父结点的指针。

## 解题思路
分为三种情况（三种情况是顺序判断的）
1. 二叉树为空，则返回空；
2. 节点右孩子存在，则设置一个指针从该节点的右孩子出发，一直沿着指向左子结点的指针找到的叶子节点即为下一个节点；
3. 节点不是根节点。如果该节点是其父节点的左孩子，则返回父节点；否则继续向上遍历其父节点的父节点，重复之前的判断，返回结果。


```C++
/*
struct TreeLinkNode {
    int val;
    struct TreeLinkNode *left;
    struct TreeLinkNode *right;
    struct TreeLinkNode *next;
    TreeLinkNode(int x) :val(x), left(NULL), right(NULL), next(NULL) {
        
    }
};
*/
class Solution {
public:
    TreeLinkNode* GetNext(TreeLinkNode* pNode)
    {
        TreeLinkNode* res = NULL;
        if(pNode == NULL){
            return NULL;
        }
        if(pNode -> right != NULL){
            TreeLinkNode* p = pNode -> right;
            while(p -> left){
                p = p -> left;
            }
            return p;
        }
        while(pNode -> next != NULL){
            TreeLinkNode* proot = pNode -> next;
            if(proot -> left == pNode){
                return proot;
            }
            pNode = pNode -> next;
        }
        return res;
    }
};
```

  