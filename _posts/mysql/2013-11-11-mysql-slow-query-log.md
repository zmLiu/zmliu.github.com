---
layout : post
title : MySql 慢查询日志
tags : [mysql]
---

打开my.cnf

找到`[mysqld]`在其下面添加

	//查询时间大于两秒的都记录
	long_query_time = 2
	//日志文件地址(必须保证启动mysql的账户对目录有写权限)
	log-slow-queries = /tmp/slow-query.log
	//记录没有使用索引的sql语句
	log-queries-not-using-indexes
	//一些管理指令,也会被记录
	log-slow-admin-statements

可以使用`mysqldumpslow`(/mysql/bin目录下)来查看日志文件,也可以直接打开日志文件查看