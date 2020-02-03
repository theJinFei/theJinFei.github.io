---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]二叉树的镜像"               # 标题  
subtitle:   "递归的使用"  #副标题 
date:       2020-1-16 10:04:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
---

## 题目描述
> 操作给定的二叉树，将其变换为源二叉树的镜像。


## 解题思路

- 这道题一直有个坑
- 判断如果当前节点的left，right节点都为空，则直接返回就行
- 如果有一个不为空，就使用一个临时变量，进行替换操作
- 然后再继续递归左右子树


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
    void Mirror(TreeNode *pRoot) {
        if(pRoot == NULL){
            return;
        }
        if(pRoot -> left == NULL && pRoot -> right == NULL){        // 注意这一点情况
            return;
        }
        TreeNode* temp = pRoot -> left;     // 这个是允许left，right节点其中有一个为空的
        pRoot -> left = pRoot -> right;
        pRoot -> right = temp;
        if(pRoot -> left){      // 把这个判空操作注释掉 也是可以过的
            Mirror(pRoot -> left);
        }
        if(pRoot -> right){
            Mirror(pRoot -> right);
        }
        
    }
};
```