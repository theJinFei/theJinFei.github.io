---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]从上往下打印二叉树"               # 标题  
subtitle:   "STL队列的使用"  #副标题 
date:       2019-12-17 23:01:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
---

## 题目描述
> 从上往下打印出二叉树的每个节点，同层节点从左至右打印。

## 解题思路

- 考察sTL中的队列
- 写法，queue<int> q,一些基本操作和stack类似，都是push，pop操作，有特殊的front
- 注意每个入队push的是，当前队列的首元素的左右节点，所以会有一个temp变量来指示。

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
    vector<int> PrintFromTopToBottom(TreeNode* root) {
        vector<int> res;
        if(root == NULL){
            return res;
        }
        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty()){
            TreeNode* temp = q.front();
            res.push_back(temp -> val);
            q.pop();
            if(temp -> left){
                q.push(temp -> left);
            }
            if(temp -> right){
                q.push(temp -> right);
            }
        }
        return res;
    }
};
```
