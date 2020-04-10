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
  
## 动态规划
- [参考](https://leetcode-cn.com/problems/nge-tou-zi-de-dian-shu-lcof/solution/nge-tou-zi-de-dian-shu-dong-tai-gui-hua-ji-qi-yo-3/)
- 通过题目我们知道一共投掷 n 枚骰子，那最后一个阶段很显然就是：当投掷完 n 枚骰子后，各个点数出现的次数。

- 注意，这里的点数指的是前 n 枚骰子的点数和，而不是第 n 枚骰子的点数，下文同理。

- 找出了最后一个阶段，那状态表示就简单了。

- 1. 首先用数组的第一维来表示阶段，也就是投掷完了几枚骰子。
- 2. 然后用第二维来表示投掷完这些骰子后，可能出现的点数。
- 数组的值就表示，该阶段各个点数出现的次数。
- 3. 所以状态表示就是这样的：dp[i][j]，表示投掷完 ii 枚骰子后，点数 jj 的出现次数。

```C++
class Solution {
public:
    vector<double> twoSum(int n) {
        int dp[15][70];
        memset(dp, 0, sizeof(dp));
        for (int i = 1; i <= 6; i ++) {
            dp[1][i] = 1;
        }
        for (int i = 2; i <= n; i ++) {
            for (int j = i; j <= 6*i; j ++) {
                for (int cur = 1; cur <= 6; cur ++) {
                    if (j - cur <= 0) {
                        break;
                    }
                    dp[i][j] += dp[i-1][j-cur];
                }
            }
        }
        int all = pow(6, n);
        vector<double> ret;
        for (int i = n; i <= 6 * n; i ++) {
            ret.push_back(dp[n][i] * 1.0 / all);
        }
        return ret;
    }
}; 
```

## 递归迭代 会出现超时

```C++
class Solution {
public:
    int countSum(int n, int sum){
        if(n < 1 || sum < n || sum > 6 * n){
            return 0;
        }
        if(n == 1){
            return 1;
        }
        int rescount = 0;
        rescount = countSum(n - 1, sum - 1) + countSum(n - 1, sum - 2) + countSum(n - 1, sum - 3) 
        + countSum(n - 1, sum - 4) + countSum(n - 1, sum - 5) + countSum(n - 1, sum - 6);
        return mmap[n];  
    }
    vector<double> twoSum(int n) {
        int total = pow(6, n);
        vector<double> res;
        for(int i = n; i <= 6 * n; i++){
            res.push_back((double)countSum(n, i) / total);
        }
        return res;
    }
};
```

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

  