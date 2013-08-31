---
layout : post
title : NativeProcess
tags : [ActionScript]
---

NativeProcess 类提供命令行集成和常规启动功能。NativeProcess 类允许 AIR 应用程序在主机操作系统上执行本机进程。AIR 应用程序可以监视进程的标准输入 (stdin) 和标准输出 (stdout) 流以及进程的标准错误 (stderr) 流。

####直接上代码(这里用adobe的ATF导出工具作为例子)
	var workingDirectory:File = File.applicationDirectory;//根据需求自己改
	var executable:File = File.applicationDirectory.resolvePath("png2atf");//根据需求自己改
	var params:Vector.<String> = new Vector.<String>();
	params.push("-c");
	params.push("-i");
	params.push("test.png");
	params.push("-o");
	params.push("test.atf");
	
	var info:NativeProcessStartupInfo = new NativeProcessStartupInfo();
	info.workingDirectory = workingDirectory;
	info.arguments = params;
	info.executable = executable;
	
	var nativeProcess:NativeProcess = new NativeProcess();
	nativeProcess.addEventListener(NativeProcessExitEvent.EXIT,onExit);
	nativeProcess.addEventListener(ProgressEvent.STANDARD_OUTPUT_DATA,onData);
	nativeProcess.addEventListener(ProgressEvent.STANDARD_ERROR_DATA,onError);
	nativeProcess.start(info);
	
	function onExit(e:NativeProcessExitEvent):void{
		trace("exit");
	}
	
	function onData(e:ProgressEvent):void{
		trace(nativeProcess.standardOutput.readUTFBytes(nativeProcess.standardOutput.bytesAvailable));
	}
	
	function onError(e:ProgressEvent):void{
		trace(nativeProcess.standardError.readUTFBytes(nativeProcess.standardError.bytesAvailable));
	}

####注意事项
	该类只能在AIR中使用 player中无法使用
	应用程序配置文件需要配置
	<application xmlns="http://ns.adobe.com/air/application/3.8">
	    ...其他配置...
	    <supportedProfiles>extendedDesktop</supportedProfiles>
	</application>

####导出注意事项
	只能导出为
	本机安装程序
	或
	具有运行时绑定的应用程序
	否则NativeProcess将会报错
	错误信息为	Error: Error #3219: The NativeProcess could not be started. 'Not supported in current profile.'