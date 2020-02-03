---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]扑克牌顺子"               # 标题  
subtitle:   "排序算法的应用"  #副标题 
date:       2020-1-19 16:28:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
---

## 题目描述
> 请实现一个函数用来找出字符流中第一个只出现一次的字符。例如，当从字符流中只读出前两个字符"go"时，第一个只出现一次的字符是"g"。当从该字符流中读出前六个字符“google"时，第一个只出现一次的字符是"l"。

## 输出描述
> 如果当前字符流没有存在出现一次的字符，返回#字符。

## 解题思路

- 由于是字符序列，可以用一个256大小的表来表示这个字符流
- talbe[256]，初始化置为-1，
- 可以用一个index，表示当前出现的位置，当第一个出现的时候，即，table[ch] = index 
- 当第二次出现的时候，可以置为-2，表示出现多次
- 这样 table[i]中的i即为最后返回的字符。
- **最后遍历table，找到table[i] >= 0 && pos = table[i]  最小的值，此时的value为第一次出现的位置。**
- 利用构造函数来进行初始化表和索引。


```C++
class Solution
{
public:
    Solution(){
        index = 0;
        for(int i = 0; i < 256; i++){
            table[i] = -1;
        }
    }
    
  //Insert one char from stringstream
    void Insert(char ch)
    {
        if(table[ch] == -1){
            table[ch] = index;
        }else if(table[ch] >= 0){
            table[ch] = -2;
        }
        index++;
    }
  //return the first appearence once char in current stringstream
    char FirstAppearingOnce()
    {
        char res = '#';
        int temp = INT_MAX;
        for(int i = 0; i < 256; i++){
            if(table[i] >= 0 && table[i] < temp){
                temp = table[i];
                res = (char)i;
            }
        }
        return res;
    }
private:
    int index = 0;
    int table[256];

};
```


## 0119解题思路

- 编程的时候 理清楚思路 然后在写并不是很困难
- 利用构造函数进行初始化 table和index（这里的index是第几个字符的意思）
- 插入的时候，如果是第一次出现，则给table[i]赋值给index；如果是第二次出现，则给table[i]赋值给-2，往后就不用管了
- 记得将index累加，处理下一个字符
- 遍历的时候，如果table[i]>=0表示出现了一次，并且在256个字符中，找到最小的一个table[i]即为最后的结果


```C++
class Solution
{
public:
    Solution(){
        index = 0;
        for(int i = 0; i < 256; i++){
            table[i] = -1;
        }
    }
  //Insert one char from stringstream
    void Insert(char ch)
    {
        if(table[ch] == -1){
            table[ch] = index;
        }else if(table[ch] >= 0){
            table[ch] = -2;
        }
        index++;
    }
  //return the first appearence once char in current stringstream
    char FirstAppearingOnce()
    {
        char res = '#';
        int temp = INT_MAX;
        for(int i = 0; i < 256; i++){
            if(table[i] >= 0 && table[i] < temp){
                temp = table[i];
                res = (char)i;
            }
        }
        return res;
    }
private:
    int table[256];
    int index = 0;
};
```