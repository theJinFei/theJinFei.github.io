---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]构建乘积数组"               # 标题  
subtitle:   "矩阵乘法"  #副标题 
date:       2019-12-08              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
---

## 题目描述
> 给定一个数组A[0,1,...,n-1],请构建一个数组B[0,1,...,n-1],其中B中的元素B[i]=A[0]*A[1]*...*A[i-1]*A[i+1]*...*A[n-1]。不能使用除法。


## 解题思路
- B[i]=A[0]*A[1]*...*A[i-1]*A[i+1]*...*A[n-1]
- 从左到右算 B[i]=A[0]*A[1]*...*A[i-1]
- 从右到左算B[i]*=A[i+1]*...*A[n-1]
- 参看图![image](https://uploadfiles.nowcoder.com/images/20160829/841505_1472459965615_8640A8F86FB2AB3117629E2456D8C652)
- 第一趟算下半三角形，算出B[i]的一半
- 第二趟算上半三角形，算出另一半
- 注意数组的下标，第一趟是从 i  = 1 -> n,  B[i] = B[i - 1] * A[i - 1]
- 第二趟是从下往上走， j = n - 2 -> 0, 先算出一个临时变量，temp *= A[j + 1] ,然后B[j] *= temp 即可


```C++
class Solution {
public:
    vector<int> multiply(const vector<int>& A) {
        
        if(A.size() == 0){
            return A;
        }
        int n = A.size();
        vector<int> B(n);
        B[0] = 1;
        for(int i = 1; i < n; i++){
            B[i] = B[i - 1] * A[i - 1];
        }
        
        int temp = 1;
        for(int j = n - 2; j >= 0; j--){
            temp *= A[j + 1];
            B[j] *= temp;
        }
        
        return B;
    }
};
```

  