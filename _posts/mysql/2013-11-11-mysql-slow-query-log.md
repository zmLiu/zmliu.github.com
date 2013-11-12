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

 - -s, 是表示按照何种方式排序，c、t、l、r分别是按照记录次数、时间、查询时间、返回的记录数来排序，ac、at、al、ar，表示相应的倒叙
 
 - -t, 是top n的意思，即为返回前面多少条的数据；
 
 - -g, 后边可以写一个正则匹配模式，大小写不敏感的；
 
***比如***

	/path/mysqldumpslow -s r -t 10 /database/mysql/slow-log
	得到返回记录集最多的10个查询。
	
	/path/mysqldumpslow -s t -t 10 -g “left join” /database/mysql/slow-log
	得到按照时间排序的前10条里面含有左连接的查询语句。


 
 
