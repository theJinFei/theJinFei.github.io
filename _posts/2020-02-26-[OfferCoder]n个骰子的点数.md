---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]n个骰子的点数"               # 标题  
subtitle:   "dp"  #副标题 
date:       2020-02-26 14:21:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
    - dp
---

## 题目描述
> 把n个骰子扔在地上，所有骰子朝上的一面的点数之和为s.输入n，打印出s的所有可能的值出现的概率。


## 解题思路
- 我们设立数组dp[i][j]，用数组中的第n个数表示骰子点数和为j的次数
- 第k次投掷骰子的数可能为1~6中的任意一个数，如果我们假设第k次投掷骰子最终所有的和为n，那么和为n的次数就为前一次投掷（第k-1次投掷）和为n-1、n-2、n-3、n-4、n-5、n-6的次数的总和。
- 同时知道第1次投掷和为1,2,3,4,5,6的次数均为1；同时第k次投掷时，和为0、1、2…k-1将不会存在；
```JAVA
public class Solution {
    /**
     * @param n an integer
     * @return a list of Map.Entry<sum, probability>
     */
    public List<Map.Entry<Integer, Double>> dicesSum(int n) {
        final int face = 6;
        final int pointNum = face * n;
        long[][] dp = new long[n+1][pointNum+1];
        for (int i=1;i<=face;i++)
            dp[1][i] = 1;
        for (int i=2;i<=n;i++) {
            for (int j=i;j<=i*face;j++) {
                for (int k=1;k<=j && k<=face;k++)
                    dp[i][j] += dp[i-1][j-k];
            }
        }
        List<Map.Entry<Integer, Double>> res = new ArrayList<>();
        double totalNum = Math.pow(face, n);
        for (int i=n;i<=pointNum;i++) 
            res.add(new AbstractMap.SimpleEntry<>(i, dp[n][i]/totalNum));
        return res;
    }
}
```

- 第一步，确定问题解的表达式。可将f(n, s) 表示n个骰子点数的和为s的排列情况总数。
- 第二步，确定状态转移方程。n个骰子点数和为s的种类数只与n-1个骰子的和有关。因为一个骰子有六个点数，那么第n个骰子可能出现1到6的点数。所以第n个骰子点数为1的话，f(n,s)=f(n-1,s-1)，当第n个骰子点数为2的话，f(n,s)=f(n-1,s-2)，…，依次类推。在n-1个骰子的基础上，再增加一个骰子出现点数和为s的结果只有这6种情况！


```C++
#include <stdio.h>
#include <string.h>

#include <iostream>
using namespace std;

/****************************
func:获取n个骰子指定点数和出现的次数
para:n:骰子个数;sum:指定的点数和
return:点数和为sum的排列数
*****************************/
int getNSumCount(int n, int sum)
{
	if(n<1||sum<n||sum>6*n)
	{
		return 0;
	}
	if(n==1)
	{
		return  1;
	}
	int resCount=0;
	resCount=getNSumCount(n-1,sum-1)+getNSumCount(n-1,sum-2)+
			 getNSumCount(n-1,sum-3)+getNSumCount(n-1,sum-4)+
			 getNSumCount(n-1,sum-5)+getNSumCount(n-1,sum-6);
	return resCount;
}

//验证
int main()
{
	int n = 0;
	while(true)
	{
		cout<<"input dice num：";
		cin>>n;
		for(int i=n;i<=6*n;++i)
		{
			cout<<"f("<<n<<","<<i<<")="<<getNSumCount(n,i)<<endl;
		}
	}
    // n 骰子数目
	int total = pow((float)6, n);
    for(int i = n; i <= 6*n; ++i)
    {
        float ratio = (float)getNSumCount(n,i)/total;  
        printf("%d: %f/n", i, ratio);  
    }  
}

```

  