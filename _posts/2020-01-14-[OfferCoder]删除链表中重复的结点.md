---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]删除链表中重复的结点"               # 标题  
subtitle:   "删除链表中的重复节点"  #副标题 
date:       2020-01-14 16:19:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
    - 数据结构
    - 链表
---

## 题目描述
> 在一个排序的链表中，存在重复的结点，请删除该链表中重复的结点，重复的结点不保留，返回链表头指针。 例如，链表1->2->3->3->4->4->5 处理后为 1->2->5




## 解题思路

- 这道题的难点是，要把所有重复的指针都给删除，而不是保留一个（所以需要一个当前节点的头一个节点pre）
- 这里需要使用一个头指针，来表示先前的一个节点（为了解决是从头就开始重复的情况）
> ListNode* pNode = new ListNode(0) <br>
pNode -> next = pTemp; <br>
------------------------------------- <br>
ListNode* pNode = NULL; <br>
pNode -> next = pTemp;  <br>
这里的区别是，第二个会报异常的，首先PNode就没有初始化值，根本不可能找到 PNode-> next ，终于想清楚了！
- 注意指针的使用


```C++
/*
struct ListNode {
    int val;
    struct ListNode *next;
    ListNode(int x) :
        val(x), next(NULL) {
    }
};
*/
class Solution {
public:
    ListNode* deleteDuplication(ListNode* pHead)
    {
        if(pHead == NULL || pHead -> next == NULL){
            return pHead;
        }
        ListNode* Head = new ListNode(0);
        Head -> next = pHead;
        ListNode* pre = Head;
        ListNode* pCur = Head -> next;
        while(pCur != NULL){
            if(pCur -> next != NULL && pCur -> next -> val == pCur -> val){
                while(pCur -> next != NULL && pCur -> next -> val == pCur -> val){
                        pCur = pCur -> next;                   
                }
                pre -> next = pCur -> next;
                pCur = pCur -> next;
            }else{
                pre = pre -> next;
                pCur = pCur -> next;
            }
        }
        return Head -> next;
    }
};
```