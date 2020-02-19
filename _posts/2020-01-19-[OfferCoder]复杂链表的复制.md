---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]复杂链表的复制"               # 标题  
subtitle:   "链表复制"  #副标题 
date:       2020-01-19 11:03:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
    - 数据结构
    - 链表
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

## 第二遍注意的点

- **注意指针的移动**
- 仔细点，指针很容易错


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
        copyNode(pHead);
        copyRandomPoint(pHead);
        RandomListNode* copyHead = splitList(pHead);
        return copyHead;
    }
    
    void copyNode(RandomListNode* pHead){
        if(pHead == NULL){
            return;
        }
        RandomListNode* p = pHead;
        while(p){
            RandomListNode* newNode = new RandomListNode(p -> label);
            newNode -> random = NULL;
            newNode -> next = p -> next;
            p -> next = newNode;
            p = newNode -> next;
        }
    }
    
    void copyRandomPoint(RandomListNode* pHead){
        if(pHead == NULL){
            return;
        }
        RandomListNode* p = pHead;
        while(p){
            RandomListNode* copyNode = p -> next;
            if(p -> random != NULL){
                copyNode -> random = p -> random -> next;
            }
            p = copyNode -> next;
        }
    }
    
    RandomListNode* splitList(RandomListNode* pHead){
        if(pHead == NULL){
            return NULL;
        }
        RandomListNode* copyHead;
        RandomListNode* copyNode;
        RandomListNode* pNode = pHead;
        if(pNode != NULL){
            copyNode = copyHead = pNode -> next;
            pNode -> next = copyNode -> next;
            pNode = pNode -> next;
        }
        while(pNode != NULL){
            copyNode -> next = pNode -> next;
            copyNode = copyNode -> next;
            pNode -> next = copyNode -> next;
            pNode = pNode -> next;
        }
        
        return copyHead;
    }
};
```

## 0119遍注意的点

- **注意指针的移动**
- 注意下面的函数

'''C++
    RandomListNode* splitList(RandomListNode* pHead){
        if(pHead == NULL){
            return NULL;
        }
        RandomListNode* pNode = pHead;
        RandomListNode* copyHead = NULL;
        RandomListNode* pcopyNode = NULL;
        if(pNode){
            copyHead = pcopyNode = pNode -> next;
            pNode -> next = pcopyNode -> next;
            pNode = pNode -> next;
        }
        while(pNode){
            pcopyNode -> next = pNode -> next;     // **指向next后，要往后移动指针，否则会置空**
            pcopyNode = pcopyNode -> next;
            pNode -> next = pcopyNode -> next;
            pNode = pNode -> next;                 // **同理**
        }
        return copyHead;
    }
'''
