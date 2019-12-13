---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]反转链表"               # 标题  
subtitle:   "链表操作  "  #副标题 
date:       2019-12-13 16:30:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
---

## 题目描述
> 输入一个链表，反转链表后，输出新链表的表头。



## 解题思路


- 需要三个指针，当更改cur->next指向前一个指针的时候，要保存next，否则链表就会段
- 定义 prev（最初赋值为NULL即可），cur（指向头节点），reverserHead(反转后的头指针)
- 循环的时候，再定义一个next，保存cur->next指针

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
    ListNode* ReverseList(ListNode* pHead) {
        if(pHead == NULL){
            return NULL;
        }
        
        ListNode* reverserHead = NULL;
        ListNode* prev = NULL;
        ListNode* cur = pHead;
        while(cur != NULL){
            ListNode* nextNode = cur -> next;
            if(nextNode == NULL){
                reverserHead = cur;
            }
            cur -> next = prev;
            prev = cur;
            cur = nextNode;
        }
        return reverserHead;
    }
};
```
