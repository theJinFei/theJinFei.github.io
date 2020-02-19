---
layout:     post                    # 使用的布局（不需要改） 
title:      "[iTools]MathType7+Word2016工具使用"               # 标题  
subtitle:   "MathType7嵌套Word"  #副标题 
date:       2019-12-11 15:22：00              # 时间 
author:     "JinFei"                    # 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       # 是否归档 
tags:                               #标签     
    - iTools 
---

## 描述

> 这两天一直在捣弄MathType7怎么嵌入Word2016环境的问题，捣弄了一晚上+一上午。。。。贼气。。。

## 环境+failed尝试方法

- 环境 windows10 + Office2016 专业版 + Mathtype7.2 （上一个版本过期了 sad...）
- 知乎上讨论了[MathType+Word2016兼容问题](https://www.zhihu.com/question/37390635)
- 需要 将路径被office信任，打开word→文件→选项→信任中心→信任中心设置→添加新位置,添加C:\Program Files\Microsoft Office\Office16\STARTUP， 如图 <br> ![加信任](https://pic1.zhimg.com/80/v2-6ee1d4e1997628b6bbbe659eca3c92b7_hd.jpg) //这个要看自己的word装在哪了，我就装的不是默认的位置
- 然后什么需要将mathtype的64位的MathPage.wll拷贝到什么什么位置。对我来说没有太大的用。

## Successed尝试办法

- 参考[ctrlV闪退的情况解决办法](http://www.mathtype.cn/wenti/wufa-fuzhizhantie.html)
- 首先还是上步failed尝试办法，将路径加入office信任，步骤如上。
- 新版MathType目录结构如图 <br> ![image](./../img/mathtype_office_support.png =100x100)，
- 将新版的MathType Commands 2016.dotm文件拷贝到Office路径下root\Office16的路径，如 C:\Program Files (x86)\Microsoft Office\root\Office16\STARTUP，这个STARTUP目录下，
- 将MathPage.wll文件拷贝到 C:\Users\username\AppData\Roaming\Microsoft\Word\STARTUP，这个应该是word为本地用户生成的目录。**（注意，这两个MathType Commands 2016.dotm，MathPage.wll应该是32位的，我也不知道为啥）**
- 搞定