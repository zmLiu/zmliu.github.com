---
layout : post
title : The last packet successfully received from the server was XXX seconds ago
tags : [Java,mysql]
---

***导致这个错误的原因是mysql `wait_timeout`***

***解决方法:*** 设置`wait_timeout`大于 `检测连接是否有效的 线程运行时间`

***比方说`wait_timeout=10秒` 那么`检测连接是否有效的 线程运行时间`就需要小于10秒***

