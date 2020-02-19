---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]按照之字形打印二叉树/把二叉树打印成多行"               # 标题  
subtitle:   "树的层次遍历，顺序打印,层次遍历"  #副标题 
date:       2019-12-18 16:34:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
    - 数据结构
    - 树
---

## 题目描述
> 请实现一个函数按照之字形打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右至左的顺序打印，第三行按照从左到右的顺序打印，其他行以此类推。


## 解题思路

- 借助队列来层次遍历
- 一个变量表示第i层，如果是奇数层，需要调用STL中的reverse函数
- **每次遍历当前队列元素个数的元素**
- 每次遍历的时候，将左右子节点入队列


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
    vector<vector<int> > Print(TreeNode* pRoot) {
        vector<vector<int> > res;
        if(pRoot == NULL){
            return res;
        }
        queue<TreeNode*> q;
        q.push(pRoot);
        int level = 0;
        while(!q.empty()){
            int size = q.size();
            vector<int> temp;
            for(int i = 0; i < size; i++){
                TreeNode* t = q.front();
                temp.push_back(t -> val);
                q.pop();
                if(t -> left != NULL){
                    q.push(t -> left);
                }
                if(t -> right != NULL){
                    q.push(t -> right);
                }
            }
            if((level & 1) != 0){
                reverse(temp.begin(), temp.end());
            }
            res.push_back(temp);
            level++;
        }
        return res;
    }
    
};
```


## 题目描述
> 从上到下按层打印二叉树，同一层结点从左至右输出。每一层输出一行。


## 解题思路

- 层次遍历二叉树是一样的
- 只是少一步判断是奇数层还是偶数层，不用逆转临时层次的vector

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
        vector<vector<int> > Print(TreeNode* pRoot) {
            vector<vector<int> > res;
            if(pRoot == NULL){
                return res;
            }
            queue<TreeNode*> q;
            q.push(pRoot);
            while(!q.empty()){
                int size = q.size();
                vector<int> temp;
                for(int i = 0; i < size; i++){
                    TreeNode* t = q.front();
                    temp.push_back(t -> val);
                    q.pop();
                    if(t -> left){
                        q.push(t -> left);
                    }
                    if(t -> right){
                        q.push(t -> right);
                    }
                }
                res.push_back(temp);
            }
            return res;
        }
    
};
```


