---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]两个链表的第一个公共结点"               # 标题  
subtitle:   "快慢指针的应用"  #副标题 
date:       2019-12-14 16:07:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
---

## 题目描述
> 输入两个链表，找出它们的第一个公共结点。



## 解题思路


- 快慢指针的使用
- 先求出两个链表的长度
- 长的链表，快指针，先走，具体步数为 长链表减去短链表的长度
- 然后同时走，直至相等
- 使用函数模板 greater<T>模板，从大到小排序

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
    ListNode* FindFirstCommonNode( ListNode* pHead1, ListNode* pHead2) {
        if(pHead1 == NULL ||pHead2 == NULL){
            return NULL;
        }
        int len1 = 0, len2 = 0;
        ListNode* p1 = pHead1;
        ListNode* p2 = pHead2;
        while(p1 != NULL){
            len1++;
            p1 = p1 -> next;
        }
        while(p2 != NULL){
            len2 ++;
            p2 = p2 -> next;
        }
        p1 = pHead1;
        p2 = pHead2;
        while(len1 > len2){
            len1--;
            p1 = p1-> next;
        }
        while(len2 > len1){
            len2--;
            p2 = p2-> next;
        }
        while(p1 != p2){
            p1 = p1 -> next;
            p2 = p2 -> next;
        }
        return p1;
    }
};
```
