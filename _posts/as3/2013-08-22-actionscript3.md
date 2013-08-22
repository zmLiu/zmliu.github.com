---
layout : post
title : iphone5 Stage 尺寸问题
tags : [ActionScript]
---

####今天用AdobeAir 实现的程序上`iTouch5`测试发现尺寸怎么获取出来都是`960*640`

####然后细细回味了一下之前的程序 貌似没有遇到这个问题。

####仔细对比了一下工程和之前工程的差别。发现唯一就是没有`icon图`和`启动图像`。果断添加了`Default-568h@2x.png启动图像` 再次测试OK了