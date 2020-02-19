---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]替换空格"               # 标题  
subtitle:   "原地更新字符串"  #副标题 
date:       2019-12-17 11:25:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
    - 字符串
---

## 题目描述
> 请实现一个函数，将一个字符串中的每个空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。


## 解题思路

- 本体考察的是原地更新字符串
- 首先统计出空格的次数
- 新字符串的索引长度为 原来的长度+空格的次数*2-1
- 旧字符串的索引长度为 长度-1
- 从最后开始处理，如果对应的是空格，则这个先对应着0，再对应着2，再对应着%（等于是反着过来的）
- 注意循环条件，首先old索引一定是>=0的，
- 不太懂这个限制条件，*newIndex > oldIndex*，如果没有这个限制条件的话，也是能过的。

```C++
class Solution {
public:
	void replaceSpace(char *str,int length) {
        int count = 0;
        for(int i = 0; i < length; i++){
            if(str[i] == ' '){
                count++;
            }
        }
        int newIndex = count * 2 + length - 1;
        int oldIndex = length - 1;
        for(; oldIndex >= 0 && newIndex > oldIndex; oldIndex--){
            if(str[oldIndex] == ' '){
                str[newIndex--] = '0';
                str[newIndex--] = '2';
                str[newIndex--] = '%';
            }else{
                str[newIndex--] = str[oldIndex];
            }
        }
	}
};
```
