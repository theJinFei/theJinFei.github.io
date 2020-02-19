---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]链表中环的入口结点"               # 标题  
subtitle:   "链表操作，快慢指针使用"  #副标题 
date:       2019-12-15 19:54:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
    - 数据结构
    - 链表
---

## 题目描述
> 给一个链表，若其中包含环，请找出该链表的环的入口结点，否则，输出null。

## 解题思路

- 快慢指针的应用
- 链表中有环的条件为，快慢指针一定会在环中相遇
- 求环的入口节点，当快慢指针相遇的位置，然后初始化一个指针指向头结点，这次以每次只走一步的步伐，最终会在环的入口节点相遇

## 第二遍注意的点

- **注意指针的移动**
- 指针的初始化位置，开始的时候，快慢指针都指向头节点
- 然后才开始，快指针一次走两步；慢指针一次走一步


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
    ListNode* EntryNodeOfLoop(ListNode* pHead)
    {
        if(pHead == NULL){
            return NULL;
        }
        ListNode *slow = pHead, * fast = pHead;
        bool isHaveCycle = false;
        while(fast != NULL && fast -> next != NULL){
            slow = slow -> next;
            fast = fast -> next -> next;
            if(slow == fast){
                isHaveCycle = true;
                break;
            }

        }
        if(isHaveCycle){
            slow = pHead;
            while(fast != slow){
                fast = fast -> next;
                slow = slow -> next;
            }
            return fast;
        }
        else{
            return NULL;
        }
        
    }
};
```

## 第三遍注意的点

- **注意指针的移动**
- 指针的初始化位置，开始的时候，快慢指针都指向头节点
- **循环的条件，fast-> next不为空也要进行判断，然后才能放心的移动指针问题**