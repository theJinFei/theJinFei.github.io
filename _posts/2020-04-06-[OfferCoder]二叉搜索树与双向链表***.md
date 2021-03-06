---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]二叉搜索树与双向链表"               # 标题  
subtitle:   "二叉搜索树、双向链表"  #副标题 
date:       2020-04-06 22:15:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
    - 数据结构
    - 树
    - 双向链表
---

## 题目描述
> 输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的双向链表。要求不能创建任何新的结点，只能调整树中结点指针的指向。

## 解题思路

- 递归思想
- 按照中序遍历的顺序，当我们遍历到根节点（值为10的节点）时，左子树已经转换成一个排序的链表了，并且处在链表中的最后一个节点是当前值最大的节点
- 把链表中的当前最大值与根节点连接起来
- 去转换右子树
- 把右子树中最小的节点连接起来

```C++
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* left;
    Node* right;

    Node() {}

    Node(int _val) {
        val = _val;
        left = NULL;
        right = NULL;
    }

    Node(int _val, Node* _left, Node* _right) {
        val = _val;
        left = _left;
        right = _right;
    }
};
*/
class Solution {
public:
    Node* head; // 这里因为子函数需要对head，tail进行修改
    Node* tail;
    Node* treeToDoublyList(Node* root) {
        if(!root) return nullptr;
        helper(root);
        head -> left = tail;    // 循环链表
        tail -> right = head;
        return head;
    }
    void helper(Node* root) {
        if(!root)  return;
        helper(root->left);
        if(!head) {
            head = root;   // 找到head
            tail = root;    // 对pre进行初始化
        } else {
            tail -> right = root;
            root -> left = tail;
            tail = root;
        }
        helper(root->right);
    }
};

```

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
    TreeNode* Convert(TreeNode* pRootOfTree)
    {
       TreeNode* pLastNodeInList = NULL;
       ConvertNode(pRootOfTree, &pLastNodeInList);
        
        // pLastNodeInList 指向双向链表的尾节点
        TreeNode* pHeadOfList = pLastNodeInList;
        while(pLastNodeInList != NULL && pHeadOfList -> left != NULL){
            pHeadOfList = pHeadOfList -> left;
        }
        return pHeadOfList;
    }
    
    void ConvertNode(TreeNode* pNode, TreeNode** pLastNodeInList){
        if(pNode == NULL){
            return;
        }
        
        TreeNode* pCurrent = pNode;
        if(pCurrent -> left != NULL){
            ConvertNode(pCurrent -> left, pLastNodeInList);
        }
        
        pCurrent -> left = *pLastNodeInList;
        if(*pLastNodeInList != NULL){
            (*pLastNodeInList) -> right = pCurrent;
        }
        
        *pLastNodeInList = pCurrent;
        if(pCurrent -> right != NULL){
            ConvertNode(pCurrent -> right, pLastNodeInList);
        }
    }
};
```

  