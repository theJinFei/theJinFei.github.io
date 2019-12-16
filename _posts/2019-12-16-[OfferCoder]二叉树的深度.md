---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]二叉树的深度"               # 标题  
subtitle:   "递归求深度"  #副标题 
date:       2019-12-16 22:59:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
---

## 题目描述
> 输入一棵二叉树，求该树的深度。从根结点到叶结点依次经过的结点（含根、叶结点）形成树的一条路径，最长路径的长度为树的深度。


## 解题思路

- 递归求树的深度

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
    int TreeDepth(TreeNode* pRoot)
    {
        if(pRoot == NULL){
            return 0;
        }
        int leftDepth = TreeDepth(pRoot -> left);
        int rightDepth = TreeDepth(pRoot -> right);
        return leftDepth > rightDepth ? leftDepth + 1 : rightDepth + 1;
    }
};
```
