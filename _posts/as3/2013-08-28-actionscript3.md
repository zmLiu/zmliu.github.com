---
layout : post
title : StageWebView 用法
tags : [ActionScript]
---

####`StageWebView` 只能再Air中使用。Player中不能使用

####代码
	var stagewebview:StageWebView = new StageWebView();
	stagewebview.loadURL("http://www.baidu.com");
	stagewebview.viewPort = new Rectangle(0,0,stage.fullScreenWidth,stage.fullScreenHeight);
	stagewebview.stage = stage;
####xml配置
	<renderMode>cpu</renderMode>

####参考连接
####[http://help.adobe.com/zh_CN/as3/dev/WS901d38e593cd1bac3ef1d28412ac57b094b-8000.html](http://help.adobe.com/zh_CN/as3/dev/WS901d38e593cd1bac3ef1d28412ac57b094b-8000.html)
