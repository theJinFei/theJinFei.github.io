---
layout:     post                    # 使用的布局（不需要改） 
title:      "[剑指Offer]孩子们的游戏(圆圈中最后剩下的数)"               # 标题  
subtitle:   "环形链表"  #副标题 
date:       2020-04-06 22:03:00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - 剑指Offer 
    - 数据结构
    - 双向链表
    - 数学
---

## 题目描述
> 每年六一儿童节,牛客都会准备一些小礼物去看望孤儿院的小朋友,今年亦是如此。HF作为牛客的资深元老,自然也准备了一些小游戏。其中,有个游戏是这样的:首先,让小朋友们围成一个大圈。然后,他随机指定一个数m,让编号为0的小朋友开始报数。每次喊到m-1的那个小朋友要出列唱首歌,然后可以在礼品箱中任意的挑选礼物,并且不再回到圈中,从他的下一个小朋友开始,继续0...m-1报数....这样下去....直到剩下最后一个小朋友,可以不用表演,并且拿到牛客名贵的“名侦探柯南”典藏版(名额有限哦!!^_^)。请你试着想下,哪个小朋友会得到这份礼品呢？(注：小朋友的编号是从0到n-1)
如果没有小朋友，请返回-1




## 解题思路


- 写一个环形列表
- 数到m时，m出局即可
- 可以用STL的list进行模拟列表
- **vecotr是不行的**




```C++
class Solution {
public:
    int LastRemaining_Solution(int n, int m)
    {
        if(n < 1 || m < 1){
            return -1;
        }
        list<int> number;
        
        for(int i = 0; i < n; i++){
            number.push_back(i);
        }
        list<int>::iterator iter = number.begin();
        while(number.size() > 1){
            for(int i = 0; i < m - 1; i++){
                iter++;
                if(iter == number.end()){
                    iter = number.begin();
                }
            }
            list<int>::iterator iter_next = ++iter;
            if(iter_next == number.end()){
                iter_next = number.begin();
            }
            iter--;
            number.erase(iter);
            iter = iter_next;
        }
        return *iter;
    }
};
```


## 公式法

- [参考](https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/solution/huan-ge-jiao-du-ju-li-jie-jue-yue-se-fu-huan-by-as/)
>       只关心最终活着那个人的序号变化

        1. 约瑟夫问题
        这个问题实际上是约瑟夫问题，这个问题描述是

        N个人围成一圈，第一个人从1开始报数，报M的将被杀掉，下一个人接着从1开始报。如此反复，最后剩下一个，求最后的胜利者。

        这个问题自己之前刷剑指的时候写过，但是今天根本想不起之前的思路了，说明没有深刻理解，为了不再犯错，这次深入理解了一遍，于是有了这篇文章

        看了很多大佬的题解，都是用数字在进行举例，看完还是有一些疑惑，直到看了底部参考资料那篇文章才豁然开朗

        这里换了个角度举例，或许会更清晰一些，欢迎大家讨论，并不吝赐教！

        2. 问题转换
        既然约塞夫问题就是用人来举例的，那我们也给每个人一个编号（索引值），每个人用字母代替

        下面这个例子是N=8 m=3的例子

        我们定义F(n,m)表示最后剩下那个人的索引号，因此我们只关系最后剩下来这个人的索引号的变化情况即可
        ![image])(https://pic.leetcode-cn.com/d7768194055df1c3d3f6b503468704606134231de62b4ea4b9bdeda7c58232f4-%E7%BA%A6%E7%91%9F%E5%A4%AB%E7%8E%AF1.png)

        从8个人开始，每次杀掉一个人，去掉被杀的人，然后把杀掉那个人之后的第一个人作为开头重新编号

        第一次C被杀掉，人数变成7，D作为开头，（最终活下来的G的编号从6变成3）
        第二次F被杀掉，人数变成6，G作为开头，（最终活下来的G的编号从3变成0）
        第三次A被杀掉，人数变成5，B作为开头，（最终活下来的G的编号从0变成3）
        以此类推，当只剩一个人时，他的编号必定为0！（重点！）
        3. 最终活着的人编号的反推
        现在我们知道了G的索引号的变化过程，那么我们反推一下
        从N = 7 到N = 8 的过程

        如何才能将N = 7 的排列变回到N = 8 呢？

        我们先把被杀掉的C补充回来，然后右移m个人，发现溢出了，再把溢出的补充在最前面

        神奇了 经过这个操作就恢复了N = 8 的排列了！

        ![image])(https://pic.leetcode-cn.com/68509352d82d4a19678ed67a5bde338f86c7d0da730e3a69546f6fa61fb0063c-%E7%BA%A6%E7%91%9F%E5%A4%AB%E7%8E%AF2.png)

        因此我们可以推出递推公式f(8,3) = [f(7, 3) + 3] \% 8f(8,3)=[f(7,3)+3]%8
        进行推广泛化，即f(n,m) = [f(n-1, m) + m] \% nf(n,m)=[f(n−1,m)+m]%n

        4. 递推公式的导出
        再把n=1这个最初的情况加上，就得到递推公式

        f(n, m) = 0, n = 1;
        f(n, m) = [f(n - 1, m) + m] % n;  n > 1
        ​	
        

        为了更好理解，这里是拿着约瑟夫环的结论进行举例解释，具体的数学证明请参考维基百科。

```C++
class Solution {
public:
    int lastRemaining(int n, int m) {
        int pos = 0; // 最终活下来那个人的初始位置
        for(int i = 2; i <= n; i++){
            pos = (pos + m) % i;  // 每次循环右移
        }
        return pos;
    }
};
```
