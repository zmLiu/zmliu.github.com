---
layout : post
title : MySQL配置文件my.cnf详解
tags : [mysql]
---

此mysql配置文件例子针对4G内存 

##`port = 3306`
	mysql使用的端口
	
##`socket = /tmp/mysqls.sock`
	socket连接文件

##`back_log = 50`
	back_log 是操作系统在监听队列中所能保持的连接数,
	队列保存了在MySQL连接管理器线程处理之前的连接.
	如果你有非常高的连接率并且出现"connection refused" 报错,
	你就应该增加此处的值.
	检查你的操作系统文档来获取这个变量的最大值.
	如果将back_log设定到比你操作系统限制更高的值,将会没有效果
	
##`#skip-networking`
	不在TCP/IP端口上进行监听.
	如果所有的进程都是在同一台服务器连接到本地的mysqld,
	这样设置将是增强安全的方法
	所有mysqld的连接都是通过Unix sockets 或者命名管道进行的.
	注意在windows下如果没有打开命名管道选项而只是用此项
	(通过 "enable-named-pipe" 选项) 将会导致mysql服务没有任何作用!

##`max_connect_errors = 10`
	MySQL 服务所允许的同时会话数的上限
	其中一个连接将被SUPER权限保留作为管理员登录.
	即便已经达到了连接数的上限.
	max_connections = 100
	每个客户端连接最大的错误允许数量,如果达到了此限制.
	这个客户端将会被MySQL服务阻止直到执行了"FLUSH HOSTS" 或者服务重启
	非法的密码以及其他在链接时的错误会增加此值.
	查看 "Aborted_connects" 状态来获取全局计数器.
	
##`table_cache = 2048`
	所有线程所打开表的数量.
	增加此值就增加了mysqld所需要的文件描述符的数量
	这样你需要确认在[mysqld_safe]中 "open-files-limit" 变量设置打开文件数量允许至少4096
	
##`#external-locking`
	允许外部文件级别的锁. 打开文件锁会对性能造成负面影响
	所以只有在你在同样的文件上运行多个数据库实例时才使用此选项(注意仍会有其他约束!)
	或者你在文件层面上使用了其他一些软件依赖来锁定MyISAM表
	
##`max_allowed_packet = 16M`
	服务所能处理的请求包的最大大小以及服务所能处理的最大的请求大小(当与大的BLOB字段一起工作时相当必要)
	每个连接独立的大小.大小动态增加

##`binlog_cache_size = 1M`
	在一个事务中binlog为了记录SQL状态所持有的cache大小
	如果你经常使用大的,多声明的事务,你可以增加此值来获取更大的性能.
	所有从事务来的状态都将被缓冲在binlog缓冲中然后在提交后一次性写入到binlog中
	如果事务比此值大, 会使用磁盘上的临时文件来替代.
	此缓冲在每个连接的事务第一次更新状态时被创建
##`max_heap_table_size = 64M`
	独立的内存表所允许的最大容量.
	此选项为了防止意外创建一个超大的内存表导致永尽所有的内存资源.


##`sort_buffer_size = 8M`
	排序缓冲被用来处理类似ORDER BY以及GROUP BY队列所引起的排序
	如果排序后的数据无法放入排序缓冲,
	一个用来替代的基于磁盘的合并分类会被使用
	查看 "Sort_merge_passes" 状态变量.
	在排序发生时由每个线程分配

##`join_buffer_size = 8M`
	此缓冲被使用来优化全联合(full JOINs 不带索引的联合).
	类似的联合在极大多数情况下有非常糟糕的性能表现,
	但是将此值设大能够减轻性能影响.
	通过 "Select_full_join" 状态变量查看全联合的数量
	当全联合发生时,在每个线程中分配

##`thread_cache_size = 8`
	我们在cache中保留多少线程用于重用
	当一个客户端断开连接后,如果cache中的线程还少于thread_cache_size,
	则客户端线程被放入cache中.
	这可以在你需要大量新连接的时候极大的减少线程创建的开销
	(一般来说如果你有好的线程模型的话,这不会有明显的性能提升.)

##`thread_concurrency = 8`
	此允许应用程序给予线程系统一个提示在同一时间给予渴望被运行的线程的数量.
	此值只对于支持 thread_concurrency() 函数的系统有意义( 例如Sun Solaris).
	你可可以尝试使用 [CPU数量]*(2..4) 来作为thread_concurrency的值


##`query_cache_size = 64M`
	查询缓冲常被用来缓冲 SELECT 的结果并且在下一次同样查询的时候不再执行直接返回结果.
	打开查询缓冲可以极大的提高服务器速度, 如果你有大量的相同的查询并且很少修改表.
	查看 "Qcache_lowmem_prunes" 状态变量来检查是否当前值对于你的负载来说是否足够高.
	注意: 在你表经常变化的情况下或者如果你的查询原文每次都不同,
	查询缓冲也许引起性能下降而不是性能提升.

##`query_cache_limit = 2M`
	只有小于此设定值的结果才会被缓冲
	此设置用来保护查询缓冲,防止一个极大的结果集将其他所有的查询结果都覆盖.

##`ft_min_word_len = 4`
	被全文检索索引的最小的字长.
	你也许希望减少它,如果你需要搜索更短字的时候.
	注意在你修改此值之后,
	你需要重建你的 FULLTEXT 索引

##`#memlock`
	如果你的系统支持 memlock() 函数,你也许希望打开此选项用以让运行中的mysql在在内存高度紧张的时候,数据在内存中保持锁定并且防止可能被swapping out
	此选项对于性能有益

##`default_table_type = MYISAM`
	当创建新表时作为默认使用的表类型,
	如果在创建表示没有特别执行表类型,将会使用此值

##`thread_stack = 192K`
	线程使用的堆大小. 此容量的内存在每次连接时被预留.
	MySQL 本身常不会需要超过64K的内存
	如果你使用你自己的需要大量堆的UDF函数
	或者你的操作系统对于某些操作需要更多的堆,
	你也许需要将其设置的更高一点.

##`transaction_isolation = REPEATABLE-READ`
	设定默认的事务隔离级别.可用的级别如下:
	READ-UNCOMMITTED, READ-COMMITTED, REPEATABLE-READ, SERIALIZABLE

##`tmp_table_size = 64M`
	内部(内存中)临时表的最大大小
	如果一个表增长到比此值更大,将会自动转换为基于磁盘的表.
	此限制是针对单个表的,而不是总和.

##`log-bin=mysql-bin`
	打开二进制日志功能.
	在复制(replication)配置中,作为MASTER主服务器必须打开此项
	如果你需要从你最后的备份中做基于时间点的恢复,你也同样需要二进制日志.

##`#log_slave_updates`
	如果你在使用链式从服务器结构的复制模式 (A->B->C),
	你需要在服务器B上打开此项.
	此选项打开在从线程上重做过的更新的日志,
	并将其写入从服务器的二进制日志.


##`#log`
	打开全查询日志. 所有的由服务器接收到的查询 (甚至对于一个错误语法的查询)
	都会被记录下来. 这对于调试非常有用, 在生产环境中常常关闭此项.

##`#log_warnings`
	将警告打印输出到错误log文件.  如果你对于MySQL有任何问题
	你应该打开警告log并且仔细审查错误日志,查出可能的原因.

##`log_slow_queries`
	记录慢速查询. 慢速查询是指消耗了比 "long_query_time" 定义的更多时间的查询.
	如果 log_long_format 被打开,那些没有使用索引的查询也会被记录.
	如果你经常增加新查询到已有的系统内的话. 一般来说这是一个好主意,

##`long_query_time = 2`
	所有的使用了比这个时间(以秒为单位)更多的查询会被认为是慢速查询.
	不要在这里使用"1", 否则会导致所有的查询,甚至非常快的查询页被记录下来(由于MySQL 目前时间的精确度只能达到秒的级别).

##`log_long_format`
	在慢速日志中记录更多的信息.
	一般此项最好打开.
	打开此项会记录使得那些没有使用索引的查询也被作为到慢速查询附加到慢速日志里


##`#tmpdir = /tmp`
	此目录被MySQL用来保存临时文件.例如,
	它被用来处理基于磁盘的大型排序,和内部排序一样.
	以及简单的临时表.
	如果你不创建非常大的临时文件,将其放置到 swapfs/tmpfs 文件系统上也许比较好
	另一种选择是你也可以将其放置在独立的磁盘上.
	你可以使用";"来放置多个路径
	他们会按照roud-robin方法被轮询使用.


##`server-id = 1`
	唯一的服务辨识号,数值位于 1 到 2^32-1之间.
	此值在master和slave上都需要设置.
	如果 "master-host" 没有被设置,则默认为1, 但是如果忽略此选项,MySQL不会作为master生效.

##`Slave`
	复制的Slave (去掉master段的注释来使其生效)
	#
	为了配置此主机作为复制的slave服务器,你可以选择两种方法:
	#
	1) 使用 CHANGE MASTER TO 命令 (在我们的手册中有完整描述) -
	   语法如下:
	
	   CHANGE MASTER TO MASTER_HOST=<host>, MASTER_PORT=<port>,
	   MASTER_USER=<user>, MASTER_PASSWORD=<password> ;
	
	   你需要替换掉 <host>, <user>, <password> 等被尖括号包围的字段以及使用master的端口号替换<port> (默认3306).
	
	   例子:
	
	   CHANGE MASTER TO MASTER_HOST='125.564.12.1', MASTER_PORT=3306,
	   MASTER_USER='joe', MASTER_PASSWORD='secret';
	
	或者
	#
	2) 设置以下的变量. 不论如何, 在你选择这种方法的情况下, 然后第一次启动复制(甚至不成功的情况下,
	   例如如果你输入错密码在master-password字段并且slave无法连接),
	   slave会创建一个 master.info 文件,并且之后任何对于包含在此文件内的参数的变化都会被忽略
	   并且由 master.info 文件内的内容覆盖, 除非你关闭slave服务, 删除 master.info 并且重启slave 服务.
	   由于这个原因,你也许不想碰一下的配置(注释掉的) 并且使用 CHANGE MASTER TO (查看上面) 来代替
	
	所需要的唯一id号位于 2 和 2^32 - 1之间
	(并且和master不同)
	如果master-host被设置了.则默认值是2
	但是如果省略,则不会生效
	#server-id = 2
	
	复制结构中的master - 必须
	#master-host = <hostname>
	
	当连接到master上时slave所用来认证的用户名 - 必须
	#master-user = <username>
	
	当连接到master上时slave所用来认证的密码 - 必须
	#master-password = <password>
	
	master监听的端口.
	可选 - 默认是3306
	#master-port = <port>

##`#read_only`
	使得slave只读.只有用户拥有SUPER权限和在上面的slave线程能够修改数据.
	你可以使用此项去保证没有应用程序会意外的修改slave而不是master上的数据

##`quick`
	[mysqldump]
	不要在将内存中的整个结果写入磁盘之前缓存. 在导出非常巨大的表时需要此项


	max_allowed_packet = 16M

	[mysql]
	no-auto-rehash

##`#safe-updates`
	仅仅允许使用键值的 UPDATEs 和 DELETEs .

---
	[isamchk]
	key_buffer = 512M
	sort_buffer_size = 512M
	read_buffer = 8M
	write_buffer = 8M

	[myisamchk]
	key_buffer = 512M
	sort_buffer_size = 512M
	read_buffer = 8M
	write_buffer = 8M

	[mysqlhotcopy]
	interactive-timeout
---

##`open-files-limit = 8192`
	[mysqld_safe]
	增加每个进程的可打开文件数量.
	警告: 确认你已经将全系统限制设定的足够高!
	打开大量表需要将此值设b




