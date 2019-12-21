---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]合并两个排序的链表"               # 标题  
subtitle:   "链表操作  "  #副标题 
date:       2019-12-13 17:11:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
---

## 题目描述
> 输入两个单调递增的链表，输出两个链表合成后的链表，当然我们需要合成后的链表满足单调不减规则。



## 解题思路


- 生成一个新的头节点
- 最后返回头节点的next指针

### 常规版本

```C++
/*
struct ListNode {
	int val;
	struct ListNode *next;
	ListNode(int x) :
			val(x), next(NULL) {
	}
};*/
class Solution {
public:
    ListNode* Merge(ListNode* pHead1, ListNode* pHead2)
    {
        if(pHead1 == NULL && pHead2 == NULL){
            return NULL;
        }
        if(pHead1 == NULL){
            return pHead2;
        }
        if(pHead2 == NULL){
            return pHead1;
        }
        ListNode* p = pHead1, *q = pHead2;
        ListNode* merge_head = new ListNode(-1);     //生成一个新的头节点，头结点的next指向合并后的第一个节点，最后返回时，返回头结点的next即可
        if(p -> val <= q -> val){
            merge_head -> next = p;
            p = p -> next;
        }else{
            merge_head -> next = q;
            q = q -> next;
        }
        ListNode* tail = merge_head -> next;
        while(p && q){
            if(p -> val <= q -> val){
                tail -> next = p;
                tail = p;
                p  = p -> next;
            }else{
                tail -> next = q;
                tail = q;
                q = q -> next;
            }
        }
        if(p != NULL){
            tail -> next = p;
        }
        if(q != NULL){
            tail -> next = q;
        }
        return merge_head -> next;
    }
};
```


### 递归版本

```C++
/*
struct ListNode {
	int val;
	struct ListNode *next;
	ListNode(int x) :
			val(x), next(NULL) {
	}
};*/
class Solution {
public:
    ListNode* Merge(ListNode* pHead1, ListNode* pHead2)
    {
        if(pHead1 == NULL && pHead2 == NULL){
            return NULL;
        }
        if(pHead1 == NULL){
            return pHead2;
        }
        if(pHead2 == NULL){
            return pHead1;
        }
        ListNode* merge_head = NULL;
        if(pHead1 -> val <= pHead2 -> val){
            merge_head = pHead1;
            merge_head -> next = Merge(pHead1 -> next, pHead2);
        }else{
            merge_head = pHead2;
            merge_head ->next = Merge(pHead1, pHead2 -> next);
        }
        return merge_head;
    }
};
```