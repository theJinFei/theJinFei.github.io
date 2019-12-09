---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]正则表达式匹配"               # 标题  
subtitle:   "正则表达式"  #副标题 
date:       2019-12-09 16:13:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
---

## 题目描述
> 请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串"+100","5e2","-123","3.1416"和"-1E-16"都表示数值。 但是"12e","1a3.14","1.2.3","+-5"和"12e+4.3"都不是。

## 解题思路


- 一个一个测试用例的判断是否符合规范，
- 比如 e不能是末尾，后面跟数字
- e只能出现一次
- +-可以出现两次，如果不是开头，需要前面有e
- 小数点也只能出现一次，**这个有一个测试用例一直过不了，后面看测试用力的时候，加了个判断是否出现了e就过了，觉得没什么道理**

```C++
class Solution {
public:
    bool isNumeric(char* string)
    {
        bool sign = false, decimal = false, hasE = false;
        for(int i = 0; i < strlen(string); i++){
            if(string[i] == 'e' || string[i] == 'E'){
                if(i == strlen(string) - 1){    // e不能是末尾，后面要跟数字
                    return false;
                }
                if(hasE){
                    return false;    // e 只能出现一次
                }
                hasE = true;
            }else if(string[i] == '+' || string[i] == '-'){
                if(sign && string[i - 1]  != 'E' && string[i - 1] != 'e'){    // 第二次出现+-符号后，则必须紧跟在e后面
                    return false;
                }
                if(!sign && i > 0 && string[i - 1] != 'e' && string[i - 1] != 'E'){    // 第一次出现的+-，不在开头，前一位也要紧跟e
                    return false;
                }
                sign = true;        // 表明是第一位出现
            }else if(string[i] == '.'){
                if(hasE || decimal){        // 小数点只能出现一次 ????这一步一个测试用例一直过不了。。
                    return false;
                }
                decimal = true;
            }else if(string[i] < '0' || string[i] > '9'){
                return false;
            }
        }
        return true;
    }
};
```

  