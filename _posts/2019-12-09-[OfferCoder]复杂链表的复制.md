---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]复杂链表的复制"               # 标题  
subtitle:   "链表复制"  #副标题 
date:       2019-12-09 12:00:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
---

## 题目描述
> 输入一个复杂链表（每个节点中有节点值，以及两个指针，一个指向下一个节点，另一个特殊指针指向任意一个节点），返回结果为复制后复杂链表的head。（注意，输出结果中请不要返回参数中的节点引用，否则判题程序会直接返回空）

## 解题思路

- 第一步，将链表各个节点复制
- 第二步，复制random指针
- 第三步，将链表拆成两个链表（头指针单独处理）

```C++
/*
struct RandomListNode {
    int label;
    struct RandomListNode *next, *random;
    RandomListNode(int x) :
            label(x), next(NULL), random(NULL) {
    }
};
*/
class Solution {
public:
    RandomListNode* Clone(RandomListNode* pHead)
    {
        CloneNode(pHead);
        ConnectRandomNode(pHead);
        return SplitTwoList(pHead);
    }
    
    // 复制各个节点
    void CloneNode(RandomListNode* pHead){
        RandomListNode* pNode = pHead;
        while(pNode != NULL){
            RandomListNode* pClone = new RandomListNode(pNode -> label);
            pClone -> label = pNode -> label;
            pClone -> next = pNode -> next;
            pClone -> random = NULL;
            pNode -> next = pClone;
            pNode = pClone -> next;
        }
    }
    // 复制随机指针
    void ConnectRandomNode(RandomListNode* pHead){
        RandomListNode* pNode = pHead;
        while(pNode != NULL){
            RandomListNode* pClone = pNode -> next;
            if(pNode -> random != NULL){
                pClone -> random = pNode -> random -> next;
            }
            pNode = pClone -> next;
        }
    }
    // 拆成两个链表
    RandomListNode* SplitTwoList(RandomListNode* pHead){
        RandomListNode* pNode = pHead;
        RandomListNode* pCloneHead = NULL;
        RandomListNode* pCloneNode = NULL;
        if(pNode != NULL){
            pCloneHead = pCloneNode = pNode -> next;
            pNode -> next = pCloneNode -> next;
            pNode = pNode -> next;
        }
        
        while(pNode != NULL){
            pCloneNode -> next = pNode -> next;
            pCloneNode = pCloneNode -> next;
            pNode -> next = pCloneNode -> next;
            pNode = pNode -> next;
        }
        return pCloneHead;

    }
    
};
```

  