---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]把字符串转换成整数"               # 标题  
subtitle:   "字符串转整数，各种异常的处理"  #副标题 
date:       2020-03-01 20:19:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
---

## 题目描述
> 将一个字符串转换成一个整数，要求不能使用字符串转换整数的库函数。 数值为0或者字符串不是一个合法的数值则返回0

## 输入描述
> 输入一个字符串,包括数字字母符号,可以为空

## 输出描述
> 如果是合法的数值表达则返回该数字，否则返回0

## 示例1
> 输入 <br>
     +2147483647 <br>
    1a33 <br>
> 输出 <br>
    2147483647 <br>
    0 <br>


## 解题思路

-  各种异常的处理过程

```C++
class Solution {
public:
    int StrToInt(string str) {
        if(str.size() == 0){
            return 0;
        }
        int i = 0;
        while(str[i] == ' '){    // 判断前面的空值
            i++;
        }
        int flag = 1;
        if(str[i] == '-' || str[i] == '+'){    // 判断正负号
            if(str[i] == '-'){
                flag = -1;
            }
            i++;
        }
        if(str[i] == '0'){    // 开头有0的情况
            return 0;
        }
        long long res = 0;
        for(; i < str.size(); i++){
            if(str[i] < '0' || str[i] > '9'){
                return 0;
            }
            res = res * 10 + str[i] - '0';
            if(res * flag > INT_MAX || res * flag < INT_MIN){    // 判断溢出
                return 0;
            }
        }
        return res * flag;
    }
};
```

```C++
class Solution {
public:
    int StrToInt(string str) {
        int symbol = 0;
        if(str.size() == 0){
            return 0;
        }
        int k = 0, len = str.size();
        while(k < len && str[k] == ' ') k++;
        if(str[k] == '+'){
            k++;
        }else if(str[k] == '-'){
            symbol = 1;
            k++;
        }
        long long res = 0;
        while(str[k] >= '0' && str[k] <= '9'){
            res = res * 10 + (str[k] - '0');
            k++;
        }
        if(str[k] != '\0'){
            return 0;
        }
        if(symbol){
            res = -res;
        }
        if(res > INT_MAX || res < INT_MIN){
            return 0;
        }
        return (int)res;
    }

};
```

  