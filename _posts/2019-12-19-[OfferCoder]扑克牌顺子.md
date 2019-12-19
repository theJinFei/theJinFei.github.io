---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]扑克牌顺子"               # 标题  
subtitle:   "排序算法的应用"  #副标题 
date:       2019-12-19 19:44:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
---

## 题目描述
> LL今天心情特别好,因为他去买了一副扑克牌,发现里面居然有2个大王,2个小王(一副牌原本是54张^_^)...他随机从中抽出了5张牌,想测测自己的手气,看看能不能抽到顺子,如果抽到的话,他决定去买体育彩票,嘿嘿！！“红心A,黑桃3,小王,大王,方片5”,“Oh My God!”不是顺子.....LL不高兴了,他想了想,决定大\小 王可以看成任何数字,并且A看作1,J为11,Q为12,K为13。上面的5张牌就可以变成“1,2,3,4,5”(大小王分别看作2和4),“So Lucky!”。LL决定去买体育彩票啦。 现在,要求你使用这幅牌模拟上面的过程,然后告诉我们LL的运气如何， 如果牌能组成顺子就输出true，否则就输出false。为了方便起见,你可以认为大小王是0。

## 解题思路

- 考察排序算法的应用
- 这题的关键意思是，如果0的个数大于gap的个数，可以组成一个顺子
- **还有一个隐藏的条件，如果这个序列里存在重复的值，就一定组不成顺子**


```C++
class Solution {
public:
    bool IsContinuous( vector<int> numbers ) {
        if(numbers.size() == 0){
            return false;
        }
        sort(numbers.begin(), numbers.end());
        int numof0 = 0;
        int index = 0;
        while(index < numbers.size() && numbers[index] == 0){
            index++;
            numof0++;
        }
        int gap = 0;
        for(int i = index; i < numbers.size() - 1; i++){
            if(numbers[i + 1] != numbers[i]){
                gap += numbers[i + 1] - numbers[i] - 1;
            }else{
                return false;
            }
            
        }
        if(numof0 >= gap){
            return true;
        }else{
            return false;
        }
    }
};
```

  