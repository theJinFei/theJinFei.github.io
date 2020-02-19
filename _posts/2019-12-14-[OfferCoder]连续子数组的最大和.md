---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]连续子数组的最大和"               # 标题  
subtitle:   "dp"  #副标题 
date:       2019-12-14 10:40:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
    - dp
---

## 题目描述
> HZ偶尔会拿些专业问题来忽悠那些非计算机专业的同学。今天测试组开完会后,他又发话了:在古老的一维模式识别中,常常需要计算连续子向量的最大和,当向量全为正数的时候,问题很好解决。但是,如果向量中包含负数,是否应该包含某个负数,并期望旁边的正数会弥补它呢？例如:{6,-3,-2,7,-15,1,2,2},连续子向量的最大和为8(从第0个开始,到第3个为止)。给一个数组，返回它的最大连续子序列的和，你会不会被他忽悠住？(子向量的长度至少是1)




## 解题思路


- dp思想
- f(n) = a[i] || f(n - 1) + a[i] (if f(i - 1) + a[i] > a[i])
- 中间保存最大变量即可

```C++
class Solution {
public:
    int FindGreatestSumOfSubArray(vector<int> array) {
        int len = array.size();
        if(len == 0){
            return -1;
        }
        int dp[len + 1];
        memset(dp, 0, sizeof(int) * (len + 1));
        dp[0] = array[0];
        int res = INT_MIN;
        for(int i = 1; i < len; i++){
            if(dp[i - 1] + array[i] > array[i]){
                dp[i] = dp[i - 1] + array[i];
            }else{
                dp[i] = array[i];
            }
            if(dp[i] > res){
                res = dp[i];
            }
        }
        return res;
        
    }
};
```

## 第二遍问题 20.01.12


- dp思想
- 递推关系式记错
- 应该是 if(dp[i - 1] + array[i] > array[i])，就是前一个加上本次的能不能抵消到本次负数的影响
- 而不是dp[i - 1] + array[i] > 0就行。。

- 用例:
- 比如测试用例[1,-2,3,10,-4,7,2,-5]
- 如果要用 dp[i - 1] + array[i] > 0， 前一个-2也会加上去，这样3的值就会少2，变成1（仍然大于0，满足条件判断），这样总体的值就会少

```C++
class Solution {
public:
    int FindGreatestSumOfSubArray(vector<int> array) {
        if(array.size() == 0){
            return 0;
        }
        int dp[array.size()];
        memset(dp, 0, sizeof(int) * (array.size()));
        int res = INT_MIN;
        res = array[0];
        dp[0] = array[0];
        for(int i = 1; i < array.size(); i++){
            if(dp[i - 1] + array[i] > array[i]){
                dp[i] = dp[i - 1] + array[i];
            }else{
                dp[i] = array[i];
            }
            if(dp[i] > res){
                res = dp[i];
            }
        }
        return res;
    }
};
```