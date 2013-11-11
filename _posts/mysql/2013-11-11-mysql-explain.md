---
layout : post
title : 详解MySQL中EXPLAIN解释命令
tags : [mysql]
---

explain显示了mysql如何使用索引来处理select语句以及连接表。可以帮助选择更好的索引和写出更优化的查询语句。

使用方法，在select语句前加上explain就可以了：
	explain select test_field from tb_test where id=1

EXPLAIN列的解释：

`table`：显示这一行的数据是关于哪张表的

`type`：这是重要的列，显示连接使用了何种类型。从最好到最差的连接类型为const、eq_reg、ref、range、indexhe和ALL

`possible_keys`：显示可能应用在这张表中的索引。如果为空，没有可能的索引。可以为相关的域从WHERE语句中选择一个合适的语句

`key`： 实际使用的索引。如果为NULL，则没有使用索引。很少的情况下，MYSQL会选择优化不足的索引。这种情况下，可以在SELECT语句中使用USE INDEX（indexname）来强制使用一个索引或者用IGNORE INDEX（indexname）来强制MYSQL忽略索引

`key_len`：使用的索引的长度。在不损失精确性的情况下，长度越短越好

`ref`：显示索引的哪一列被使用了，如果可能的话，是一个常数

`rows`：MYSQL认为必须检查的用来返回请求数据的行数

`Extra`：关于MYSQL如何解析查询的额外信息