---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]二叉树中和为某一值的路径"               # 标题  
subtitle:   "DFS查找路径"  #副标题 
date:       2020-02-23 17:14:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
    - DFS
--- 

## 题目描述
> 输入一颗二叉树的根节点和一个整数，打印出二叉树中结点值的和为输入整数的所有路径。路径定义为从树的根结点开始往下一直到叶结点所经过的结点形成一条路径。(注意: 在返回值的list中，数组长度大的数组靠前)



## 解题思路
- 递归先序遍历树， 把结点加入路径。
- 若该结点是叶子结点则比较当前路径和是否等于期待和。
- 弹出结点，每一轮递归返回到父结点时，当前路径也应该回退一个结点
- **要把这种dfs方法记住**

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
        if(root == NULL){
            return {};
        }
        vector<vector<int>> res;
        vector<int> path;
        
        dfsFindPath(root, expectNumber, res, path);
        return res;
    }
    void dfsFindPath(TreeNode*root, int target, vector<vector<int>>& res, vector<int>& path){
        path.push_back(root -> val);
        if(root -> left == NULL && root -> right == NULL){
            if(target == root -> val){
                res.push_back(path);
            }
        }
        if(root -> left != NULL){
            dfsFindPath(root -> left, target - root -> val, res, path);
        }
        if(root -> right != NULL){
            dfsFindPath(root -> right, target - root -> val, res, path);
        }
        path.pop_back();
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
    vector<vector<int> > FindPath(TreeNode* root,int expectNumber) {
        vector<vector<int>> res;
        vector<int> path;
        if(root == NULL){
            return {};
        }
        int cur = 0;
        fun_helper(root, expectNumber, res, path, cur);
        return res;
    }
    void fun_helper(TreeNode* root, int target, vector<vector<int>>& res, vector<int>& path, int& cur){
        cur += root -> val;
        path.push_back(root -> val);
        if(root -> left == NULL && root -> right == NULL){
            if(cur == target){
                res.push_back(path);
            }
            // return;
        }
        if(root -> left != NULL){
            fun_helper(root -> left, target, res, path, cur);
        }
        if(root -> right != NULL){
            fun_helper(root -> right, target, res, path, cur);
        }
        cur -= root -> val;
        path.pop_back();
    }
    
};
```
