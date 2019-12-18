---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]树的子结构"               # 标题  
subtitle:   "判断B是不是A的子树，两个递归的使用"  #副标题 
date:       2019-12-18 15:17:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
---

## 题目描述
> 输入一棵二叉树，判断该二叉树是否是平衡二叉树。


## 解题思路

- 可以用递归的思路，先写一个helper函数，递归求树的高度
- 然后递归求，当前root节点的左右子树，看绝对值是否大于1
- 如果大于1，肯定不是
- 如果是的话，则递归判断root的左子树，root右子树是否满足平衡二叉树的性质。
- **注意一下递归的结束的条件，当root节点为NULL时，是返回true的**


```C++
class Solution {
public:
    int fun_treeDepth(TreeNode* root){
        if(root == NULL){
            return 0;
        }
        int left = fun_treeDepth(root -> left);
        int right = fun_treeDepth(root -> right);
        return left > right ? left + 1 : right + 1;
    }
    bool IsBalanced_Solution(TreeNode* pRoot) {
        if(pRoot == NULL){
            return true;
        }
        if(abs(fun_treeDepth(pRoot -> left) - fun_treeDepth(pRoot -> right)) > 1){
            return false;
        }
        bool left = IsBalanced_Solution(pRoot -> left);
        bool right = IsBalanced_Solution(pRoot -> right);
        if(left && right){
            return true;
        }else{
            return false;
        }
    }
};
```
