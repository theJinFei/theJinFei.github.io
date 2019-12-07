---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]二叉搜索树的第k个结点"               # 标题  
subtitle:   "二叉搜索树、中序遍历"  #副标题 
date:       2019-12-07              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
---

## 题目描述

> 给定一棵二叉搜索树，请找出其中的第k小的结点。例如， （5，3，7，2，4，6，8）    中，按结点数值大小顺序第三小结点的值为4。

## 解题思路
- 中序遍历
- 每次遍历后进入序列
- 得到的序列第k个即为所求
- 注意异常，比如k的size问题，空指针问题


```C++
/*
struct TreeNode {
    int val;
    struct TreeNode *left;
    struct TreeNode *right;
    TreeNode(int x) :
            val(x), left(NULL), right(NULL) {
    }
};
*/
class Solution {
public:
    TreeNode* KthNode(TreeNode* pRoot, int k)
    {
        if(pRoot == NULL || k <= 0){
            return NULL;
        }
        vector<TreeNode *> v;
        inOrder(pRoot, v);
        if(k > v.size()){
            return NULL;
        }
        return v[k - 1];
    }
    void inOrder(TreeNode* pRoot, vector<TreeNode *>& v){
        
        if(pRoot -> left){
            inOrder(pRoot -> left, v);
        }
        v.push_back(pRoot);
        if(pRoot -> right){
            inOrder(pRoot -> right, v);
        }
    }

    
};
```

  