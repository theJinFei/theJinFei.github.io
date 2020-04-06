---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]表示数值的字符串"               # 标题  
subtitle:   "字符串"  #副标题 
date:       2020-04-06 16:51:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
    - 字符串
---

## 题目描述
> 请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串"+100","5e2","-123","3.1416"和"-1E-16"都表示数值。 但是"12e","1a3.14","1.2.3","+-5"和"12e+4.3"都不是。

## 解题思路


- 一个一个测试用例的判断是否符合规范，
- 比如 e不能是末尾，后面跟数字
- e只能出现一次
- +-可以出现两次，如果不是开头，需要前面有e
- 小数点也只能出现一次，**这个有一个测试用例一直过不了，后面看测试用力的时候，加了个判断是否出现了e就过了，觉得没什么道理**
  
## 测试样例更改，一直有过不了的
```C++
/*class Solution {
public:
    bool isNumber(string s) {
        //去头尾空格
        s.erase(0,s.find_first_not_of(' '));
        s.erase(s.find_last_not_of(' ')+1);
        if(s.empty()){
            return false;
        }

        bool sign = false;
        bool hasE = false;
        bool decimal = false;
        for(int i = 0; i < s.size(); i++){
            if(s[i] == 'e' || s[i] == 'E'){
                if(i == 0 || i == s.size() - 1){
                    return false;
                }
                if(hasE){
                    return false;
                }
                hasE = true;
            }else if(s[i] == '+' || s[i] == '-'){
                if(sign && (s[i - 1] != 'e' || s[i - 1] != 'E')){
                    return false;
                }
                if(!sign && (s[i - 1] != 'e' || s[i - 1] != 'E')){
                    return false;
                }
                sign = true;
            }else if(s[i] == '.'){
                if(i == 0 || decimal){
                    return false;
                }
                decimal = true;
            }else if(s[i] == ' '){
                continue;
            }else if(s[i] < '0' || s[i] > '9'){
                return false;
            }
        }
        return true;
    }
};*/

class Solution {
public:
    bool pos_neg=1,power=1,point=1;     // 可用的符号：pos_neg是正负号,power是指数符,point是小数点
    bool isNumber(string s) {
        if(s.empty()) return 0;     // 啥?空的?还不快滚!
        else if(s[0]=='-' || s[0]=='+'){     // 正负号?没事,前面没有正负号和小数点就行
            if(pos_neg && s.length()>1 && ((s[1]>='0' && s[1]<='9') || s[1]=='.')){
                pos_neg=0;
                return isNumber(string(s,1));
            }
            else return 0;
        }
        else if(s[0]=='.'){     // 小数点?没事,前面没有小数点和指数符就行
            if(point && power && s.length()>1 && s[1]>='0' && s[1]<='9')
                return isNumber('0'+s);
            else return 0;
        }
        else if(s[0]==' '){     // 空格?没事,空格的尽头递归一遍就行
            int idx=0;
            while(idx<s.length() && s[idx]==' ')
                ++idx;
            return isNumber(string(s,idx));
        }
        else if(s[0]<'0'||s[0]>'9') // 啥?不是正负不是小数不是空格,还不是数字???还不快滚!
            return 0;
        for(int i=1;i<s.length();++i){
            if(s[i]>='0' && s[i]<='9');     // 数字?下一个
            else if(s[i]=='e'||s[i]=='E')  // 指数不允许后面有小数点和另一个指数,但允许正负号
                if(power){
                    power=0,pos_neg=1,point=0;
                    return isNumber(string(s,i+1));
                }
                else return 0;
            else if(s[i]=='.')     // 小数点不允许另一个小数点,而且小数点前面不能有指数
                if(point && power)
                    point=0;
                else return 0;
            else if(s[i]==' '){     // 空格不允许出现在串中
                while(i<s.length() && s[i]==' ') ++i;
                return i==s.length();
            }
            else return 0;
        }
        return 1;
    }
};

```

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

  