---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]对称的二叉树"               # 标题  
subtitle:   "拆分，递归的使用"  #副标题 
date:       2019-12-18 15:44:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
    - 数据结构
    - 树
---

## 题目描述
> 请实现一个函数，用来判断一颗二叉树是不是对称的。注意，如果一个二叉树同此二叉树的镜像是同样的，定义其为对称的。


## 解题思路

- 可以用递归的思路，写一个helper函数，*注意这个helper函数是两个参数*，就像二叉树的当前节点的左右子节点一样
- 递归结束条件，如果当前两个节点都为空则返回true
- 如果两个节点其中有一个为空，则返回false，肯定不相等
- 如果两个节点值不相等，则返回false
- **注意一下递归的结束的条件，当两个root节点为NULL时，是返回true的**


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
    bool isSymmetrical(TreeNode* pRoot)
    {
        int flag = fun_helper(pRoot, pRoot);
        return flag;
    }
    bool fun_helper(TreeNode* pRoot1 ,TreeNode* pRoot2){
        if(pRoot1 == NULL && pRoot2 == NULL){
            return true;
        }
        if((pRoot1 != NULL && pRoot2 == NULL) || (pRoot1 == NULL && pRoot2 != NULL)){
            return false;
        }
        
        if(pRoot1 -> val != pRoot2 -> val){
            return false;
        }
        
        return fun_helper(pRoot1 -> left, pRoot2 -> right) && fun_helper(pRoot1 -> right, pRoot2 -> left);
    }
    

};
```
