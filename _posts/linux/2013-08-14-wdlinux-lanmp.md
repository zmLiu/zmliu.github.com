---
layout : post
title : wdlinux lanmp一键包安装使用心得
tags : [linux]
---

#####官网:[wdlinux](http://www.wdlinux.cn)
#####一键包地址:[lanmp](http://www.wdlinux.cn/lanmp/) 官网的最新版不是很稳定 所以还是使用这个

###安装
	如果没有GCC或编译环境,先执行
	yum install -y gcc gcc-c++ make autoconf libtool-ltdl-devel gd-devel freetype-devel libxml2-devel libjpeg-devel libpng-devel openssl-devel curl-devel patch libmcrypt-devel libmhash-devel ncurses-devel sudo bzip2 

	wget http://dl.wdlinux.cn:5180/lanmp_laster.tar.gz
	tar zxvf lanmp_laster.tar.gz
	sh in.sh

###PHP扩展
	php.ini extension_dir = "./"为#extension_dir = "./"

###wdcp mysql 优化方案 - innodb
	需要先安装 mysql innodb
	安装方法
	wget -c http://down.wdlinux.cn/in/mysql_innodb_ins.sh
	chmod 755 mysql_innodb_ins.sh 
	./mysql_innodb_ins.sh
	
###相关服务的操作 命令
	service wdapache start|stop|restart  wdcp后台
	service nginxd start|stop|restart       nginx服务
	service httpd start|stop|restart         httpd服务
	service pureftpd start|stop|restart    ftp服务
	service mysqld start|stop|restart       mysql服务
###ftp
	如果ftp要访问/www/web以外的其他目录
	需要使用
	chown -R www 目录名
	给予wwww用户xxx目录的操作权限
	但是千万不要使用chown -R www / 会把服务器环境搞崩溃
	例如需要/home的操作权限 执行命令 chown -R www /home

#####关于一键安装包，目录，启动，lnamp,wdcp所用端口的说明
#####[http://www.wdlinux.cn/bbs/thread-192-1-1.html](http://www.wdlinux.cn/bbs/thread-192-1-1.html)

#####mysql数据迁移
#####[http://www.wdlinux.cn/bbs/thread-1521-1-1.html](http://www.wdlinux.cn/bbs/thread-1521-1-1.html)
	
#####常见问题
#####[http://www.wdlinux.cn/bbs/thread-1450-1-1.html](http://www.wdlinux.cn/bbs/thread-1450-1-1.html)

#####PHP升级脚本
#####5.3[http://www.wdlinux.cn/bbs/thread-3737-1-1.html](http://www.wdlinux.cn/bbs/thread-3737-1-1.html)
#####5.4[http://www.wdlinux.cn/bbs/thread-4771-1-2.html](http://www.wdlinux.cn/bbs/thread-4771-1-2.html)



