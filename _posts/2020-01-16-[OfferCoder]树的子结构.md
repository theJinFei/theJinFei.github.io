---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]树的子结构"               # 标题  
subtitle:   "判断B是不是A的子树，两个递归的使用"  #副标题 
date:       2020-1-16 9:35:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
    - 树
    - 数据结构
---

## 题目描述
> 输入两棵二叉树A，B，判断B是不是A的子结构。（ps：我们约定空树不是任意一个树的子结构）


## 解题思路

- 递归的使用，判断B是不是A的子结构
- 需要用两个递归函数，第一个递归的意思是，如果当前节点pRoot1 == pRoot2,则需要从该节点往下递归查找
- 第二个递归，表示如果不等，需要判断pRoot1的左子树，右子树
- 注意第二个递归的结束的条件，需要先判断pRoot2是否为空，如果pRoot2先走到空，证明pRoot2子树全部遍历完成，即都与pRoot1相匹配，返回true
- 如果pRoot1为空，证明pRoot1先走完了全部节点，返回false
- 如果pRoot1 -> val != pRoot2 -> val,也返回False
- 下面直接return pRoot1和pRoot2的左子树，右子树是否相等。


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
    bool HasSubtree(TreeNode* pRoot1, TreeNode* pRoot2)
    {
        bool res = false;
        if(pRoot1 == NULL && pRoot2 == NULL){
            return false;
        }
        if(pRoot1 != NULL && pRoot2 != NULL){
            if(pRoot1 -> val == pRoot2 -> val){
                res = DoesTree1HaveTree2(pRoot1, pRoot2);
            }
            if(!res){
                res = HasSubtree(pRoot1 -> left, pRoot2);
            }
             if(!res){
                 res = HasSubtree(pRoot1 -> right, pRoot2);
             }
        }
        return res;
    }
    bool DoesTree1HaveTree2(TreeNode* pRoot1, TreeNode* pRoot2){
        if(pRoot2 == NULL){
            return true;
        }
        if(pRoot1 == NULL){
            return false;
        }
        if(pRoot1 -> val != pRoot2 -> val){
            return false;
        }
        return DoesTree1HaveTree2(pRoot1 -> left, pRoot2 -> left) && 
            DoesTree1HaveTree2(pRoot1 -> right, pRoot2 -> right);
    }

};
```



## 0116解题思路

- 首先一个递归是判断Tree1的全部，相当于从根节点全部遍历一遍（树的先序遍历）
- 每遍历到一个根节点的时候，就进行一次对比，拿当前节点与Tree2节点对比
- 第二个递归就是遍历Tree1的当前节点与Tree2的当前节点是否相等

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
    bool HasSubtree(TreeNode* pRoot1, TreeNode* pRoot2)
    {
        bool res = false;
        if(pRoot1 == NULL || pRoot2 == NULL){
            return NULL;
        }
        res = DoesTree1HaveTree2(pRoot1, pRoot2);
        if(!res){
            res = HasSubtree(pRoot1 -> left, pRoot2);
        }
        if(!res){
            res = HasSubtree(pRoot1 -> right, pRoot2);
        }
        return res;
    }
    
    bool DoesTree1HaveTree2(TreeNode* pRoot1, TreeNode* pRoot2){
        if(pRoot2 == NULL){
            return true;
        }
        if(pRoot1 == NULL){
            return false;
        }
        if(pRoot1 -> val != pRoot2 -> val){
            return false;
        }
        return DoesTree1HaveTree2(pRoot1 -> left, pRoot2 -> left) && DoesTree1HaveTree2(pRoot1 -> right, pRoot2 -> right);
    }
};
```