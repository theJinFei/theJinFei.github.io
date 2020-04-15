---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]二叉搜索树的第k个结点"               # 标题  
subtitle:   "二叉搜索树、中序遍历"  #副标题 
date:       2020-04-15 10:04:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
    - 数据结构
    - 树
---

## 题目描述

> 给定一棵二叉搜索树，请找出其中的第k小的结点。例如， （5，3，7，2，4，6，8）中，按结点数值大小顺序第三小结点的值为4。

## 解题思路
- 首先建树
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

## 第二遍存在的问题
- 中序遍历存在问题，不能够正确遍历
- 递归结束的条件，pRoot == NULL
- 段错误，数组访问问题，k当等于0的时候，访问data[k-1]会触发段错误

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
        if(pRoot == NULL){
            return;
        }
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

## 0117第三遍存在的问题
- 还是存在上述的问题 mmp
- 中序遍历存在问题，不能够正确遍历
- 递归结束的条件，pRoot == NULL
- 段错误，数组访问问题，k当等于0的时候，访问data[k-1]会触发段错误

```C++
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
    int count = 0;
    TreeNode* res = nullptr;
    TreeNode* KthNode(TreeNode* pRoot, int k)
    {
        if(pRoot != nullptr){
            KthNode(pRoot -> left, k);
            count++;
            if(count == k){
                res = pRoot;
            }
            KthNode(pRoot -> right, k);
        }
        return res;
    }
};
```

## 第k大
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
    int count = 0;
    TreeNode* KthNode(TreeNode* pRoot, int k)
    {
        if(pRoot != nullptr){
            TreeNode* r1 = KthNode(pRoot -> right, k);
            if(r1 != nullptr){
                return r1;
            }
            count++;
            if(count == k){
                return pRoot;
            }
            TreeNode* r2 = KthNode(pRoot -> left, k);
            if(r2 != nullptr){
                return r2;
            }
        }
        return nullptr;
    }
};
```