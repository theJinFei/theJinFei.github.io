---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]翻转单词顺序列"               # 标题  
subtitle:   "字符串翻转"  #副标题 
date:       2020-04-06 16:13:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
---

## 题目描述
> 牛客最近来了一个新员工Fish，每天早晨总是会拿着一本英文杂志，写些句子在本子上。同事Cat对Fish写的内容颇感兴趣，有一天他向Fish借来翻看，但却读不懂它的意思。例如，“student. a am I”。后来才意识到，这家伙原来把句子单词的顺序翻转了，正确的句子应该是“I am a student.”。Cat对一一的翻转这些单词顺序可不在行，你能帮助他么？

## 解题思路
- 借助辅助栈
- 如果遇到的不是空格，字符串累加
- 如果遇到的是空格，则压栈即可
- 注意最后一个元素当i=str.size()-1时，由于后面没有空格了，需要单独进行处理
- 出栈的时候，可以先单独出一个元素
- 然后格式化输出，记得每个单词后有一个空格

```C++
class Solution {
public:

    string reverseWords(string s) {
        string res;
        string temp;
        for(int i = 0; i < s.size(); i++){
            if(s[i] == ' '){
                fun_reverse(temp);
                res += temp;
                res += " ";
                temp = "";
            }else{
                temp += s[i];
            }
        }
        fun_reverse(temp);
        res += temp;
        fun_reverse(res);
        string t;
        int pos = 0;
        while(pos < res.size()){    
            if(res[pos] == ' '){
                pos++;
            }else{
                break;
            }
        }
        res = res.substr(pos);  // 去掉前面的空格
        pos = res.size() - 1;   
        while(pos >= 0){   
            if(res[pos] == ' '){
                pos--;
            }else{
                break;
            }
        }
        res = res.substr(0, pos + 1);   // 去掉后面的空格
        for(int i = 0; i < res.size();){
            if(res[i] != ' '){
                t += res[i];
                i++;
            }else{
                while(res[i] == ' '){
                    i++;
                }
                t += " ";   // 去掉中间的空格
            }
        }
        return t;
    }
    void fun_reverse(string& s){
        for(int i = 0; i < s.size() / 2; i ++){
            char c = s[i];
            s[i] = s[s.size() - i - 1];
            s[s.size() - i - 1] = c;
        }
    }
};
```

```C++
class Solution {
public:
    string ReverseSentence(string str) {
        stack<string> s;
        string temp = "";
        for(int i = 0; i < str.size(); i++){
             if(str[i] != ' '){
                 temp += str[i];
             }else{
                 s.push(temp);
                 temp = "";
             }
            if(i == str.size() - 1){
                s.push(temp);
            }
        }
        
        string res = "";
        if(!s.empty()){
            res = res + s.top();
            s.pop();
            while(!s.empty()){
                res = res + " " + s.top();
                s.pop();
            }
        }

        return res;
    }
};
```

  