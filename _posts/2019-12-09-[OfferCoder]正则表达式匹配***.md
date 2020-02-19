---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]正则表达式匹配"               # 标题  
subtitle:   "正则表达式"  #副标题 
date:       2019-12-09 15:40:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
---

## 题目描述
> 请实现一个函数用来匹配包括'.'和'*'的正则表达式。模式中的字符'.'表示任意一个字符，而'*'表示它前面的字符可以出现任意次（包含0次）。 在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串"aaa"与模式"a.a"和"ab*ac*a"匹配，但是与"aa.a"和"ab*a"均不匹配

## 解题思路

1. 当模式中的第二个字符是'*'时：
    - 如果字符串第一个字符跟模式第一个字符不匹配，则模式后移2个字符，继续匹配。
    - 如果字符串第一个字符跟模式第一个字符匹配，可以有3种匹配方式：
        1. 模式后移2字符，相当于x*被忽略；
        2. 字符串后移1字符，模式后移2字符；
        3. 字符串后移1字符，模式不变，即继续匹配字符下一位，因为*可以匹配多位。
2. 当模式中的第二个字符不是“*”时：
    - 如果字符串第一个字符和模式中的第一个字符相匹配，那么字符串和模式都后移一个字符，然后匹配剩余的。
    - 如果 字符串第一个字符和模式中的第一个字符相不匹配，直接返回false。

```C++
class Solution {
public:
    bool match(char* str, char* pattern)
    {
        if(str == NULL || pattern == NULL){
            return false;
        }
        return isMatch(str, pattern);
    }
    bool isMatch(char* str, char* pattern){
        if(*str == '\0' && *pattern == '\0'){
            return true;
        }
        if(*str != '\0' && *pattern == '\0'){
            return false;
        }
        
        // 处理下一个字符为'*'的情况
        if(*(pattern + 1) == '*'){
            if(*str == *pattern || *pattern == '.' && *str != '\0'){
                return isMatch(str, pattern + 2) ||         // 1.1
                    isMatch(str + 1, pattern + 2) ||        // 1.2
                    isMatch(str + 1, pattern);              // 1.3
            }else{
                return isMatch(str, pattern + 2);           // 1.0
            }
        }
        // 当模式中的第二个字符不是'*'时,如果相匹配，则字符串和模式都后移一个字符
        if(*str == *pattern || (*pattern == '.' && *str != '\0')){  // 2
            return isMatch(str + 1, pattern + 1);
        }
        return false;
    }
};
```

  