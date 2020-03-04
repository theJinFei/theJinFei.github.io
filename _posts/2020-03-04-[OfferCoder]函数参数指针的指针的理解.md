---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]函数参数指针的指针的理解"               # 标题  
subtitle:   "指针"  #副标题 
date:       2020-03-04 15:28:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - C 
---

## 指针的指针的


```C++
#include <iostream>

using namespace std;

void funTest1(char* p){
    p++;    // 形参 p 是 实参的副本， 形参p的改变并不会改变实参的指向
}

void funTest2(char** p){
    (*p)++; // 遍历str，可以使用指针的指针，通过复制指针的指针  p即为r的副本，*p即为实参q的值，然后q的值进行累加，即可实现操作
}

void funTest3(char** p){
    (*p)++;
}

int main()
{
    char array[6] = {'a', 'b', 'c', 'd', 'e'};
    char* p = array;
    funTest1(p);
    cout << *p << endl;
    
    char array2[6] = {'a', 'b', 'c', 'd', 'e'};
    char* q = array2;
    funTest2(&q); // 相当于 char **r = q; funTest2(r);
    cout << *q << endl;

    char array3[6] = {'a', 'b', 'c', 'd', 'e'};
    char* s = array2;
    char** r =&s;
    funTest3(r);
    cout << *s << endl;

    return 0;
}

```
