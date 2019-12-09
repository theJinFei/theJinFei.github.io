---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]序列化二叉树"               # 标题  
subtitle:   "序列化，反序列化，二叉树"  #副标题 
date:       2019-12-09 20:28:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
---

## 题目描述
> 请实现两个函数，分别用来序列化和反序列化二叉树
> 二叉树的序列化是指：把一棵二叉树按照某种遍历方式的结果以某种格式保存为字符串，从而使得内存中建立起来的二叉树可以持久保存。序列化可以基于先序、中序、后序、层序的二叉树遍历方式来进行修改，序列化的结果是一个字符串，序列化时通过 某种符号表示空节点（#），以 ！ 表示一个结点值的结束（value!）。
> 二叉树的反序列化是指：根据某种遍历顺序得到的序列化字符串结果str，重构二叉树。

## 解题思路

- 序列化、反序列化递归考虑
- 参考  [nowcoder](https://www.nowcoder.com/questionTerminal/cf7e25aa97c04cc1a68c8f040e71fb84)

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
    char* Serialize(TreeNode *root) {   
        if(root == NULL){
            return NULL;
        }
        string str;
        fun_helperSerial(root, str);
        
        char* res = new char[str.length() + 1];
        int i;
        for(i = 0; i < str.length(); i++){
            res[i] = str[i];
        }
        res[i] = '\0';
        return res;
    }
    void fun_helperSerial(TreeNode* root, string& str){
        if(root == NULL){
            str += '#';
            return;
        }
        string r = to_string(root -> val);
        str += r;
        str += ',';
        fun_helperSerial(root -> left, str);
        fun_helperSerial(root -> right, str);
    }
    TreeNode* Deserialize(char *str) {
        if(str == NULL){
            return NULL;
        }
        TreeNode* res = fun_helperDeSerial(&str);
        return res;
    }
    
    TreeNode* fun_helperDeSerial(char** str){
        if(**str == '#'){
            ++(*str);
            return NULL;
        }
        int num = 0;
        while(**str != '\0' && **str != ','){
            num = num * 10 + ((**str) - '0');
            ++(*str);
        }
        TreeNode* root = new TreeNode(num);
        if(**str == '\0'){
            return root;
        }else{
            (*str)++;
        }
        root -> left = fun_helperDeSerial(str);
        root -> right = fun_helperDeSerial(str);
        return root;
    }
};
```

  