---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]二叉树中结点值的和为输入整数的所有路径"               # 标题  
subtitle:   "二叉树指定值"  #副标题 
date:       2019-12-07              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
---

## 题目描述

> 输入一颗二叉树的跟节点和一个整数，打印出二叉树中结点值的和为输入整数的所有路径。路径定义为从树的根结点开始往下一直到叶结点所经过的结点形成一条路径。(注意: 在返回值的list中，数组长度大的数组靠前)

## 解题思路
- 用一个vector代表当前路径
- 检查当前节点是否是叶子节点
- 如果是叶子节点，判断是否等于给定的值
- 如果不是，递归的查找
- 返回时，记得把当前vector的元素给弹出


```C++
/*
struct TreeNode {
	int val;
	struct TreeNode *left;
	struct TreeNode *right;
	TreeNode(int x) :
			val(x), left(NULL), right(NULL) {
	}
};*/
class Solution {
public:
    vector<vector<int> > FindPath(TreeNode* root,int expectNumber) {
        vector<vector<int> > res;
        vector<int> path;
        if(root == NULL){
            return res;
        }
        
        FindPath(root, expectNumber, 0, res, path);
        return res;
    }
    void FindPath(TreeNode* root, int expectNumber, int currentNumber, vector<vector<int> >& res, vector<int>& path){
        currentNumber += root -> val;
        path.push_back(root -> val);
        bool isleaf = root -> left == NULL && root -> right == NULL;
        if(isleaf && currentNumber == expectNumber){
            res.push_back(path);
        }
        if(root -> left){
            FindPath(root -> left, expectNumber, currentNumber, res, path);
        }
        if(root -> right){
            FindPath(root -> right, expectNumber, currentNumber, res, path);
        }
        path.pop_back();
    }
};
```

  