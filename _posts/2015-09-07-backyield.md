---
layout: post
title: "后端交接文档"
description: ""
category: 
tags: []
---
{% include JB/setup %}

###ssh
使用天宇发过来的rsa进行登录
``` ssh -i ~/.ssh/weico_rsa root@xxx.xxx.xxx.xxx ```
chmod 600 weico_rsa 你要改下权限

###服务器
1.  Linode 

> ``` ssh -i ~/workspace/weico_rsa root@106.185.52.238 -p 1010 ```
		weiconote海外


###工具
1. 压缩命令 tar

	> 压缩muma_web，文件名为web.tar.gz，压缩方式为gzip
	``` tar -czvf web.tar.gz muma_web ```

2. 复制命令

	> 本地到远程
	```
	scp -i ~/workspace/weico_rsa temp.txt deployer@42.62.2.58:/home/deployer/temp.txt
	```
	颠倒一下 远程到本地
	```
	scp -i ~/workspace/weico_rsa deployer@42.62.2.58:/home/deployer/temp.txt 1.txt
	```

3. ssh服务器端 

	> 弄个ssh-copy-id
	``` ssh-copy-id -i ~/.ssh/id_rsa.pub user@server ```
类似这样，把你的key全部加一下

4. ps查看所有用户运行的程序，和top类似，但是只查看一瞬间

	> <pre>
	[weico3@localhost project]$ ps aux|grep deployer
	deployer  5786  0.0  0.0 108460  1008 pts/2    Ss+  Jul06   0:00 -bash
	deployer  6427  0.0  0.0  28916  1256 ?        Ss   Mar09  68:08 tmux new -s devel
	deployer  6428  0.0  0.0 108460     0 pts/7    Ss   Mar09   0:00 -bash
	deployer  6445  0.0  0.0 199840   104 pts/7    S+   Mar09   0:00 mysql -uroot
	weico3   13004  0.0  0.0 103260   592 pts/1    S+   15:50   0:00 grep deployer
		</pre>
5. mysql -uroot -p是什么命令

	>这个是登mysql登陆数据库的命令，如果你使用黑窗口(DOS)的话就要这个命令进入数据库。

6. ll 命令

	> ll 命令列出的信息更加详细，有时间，是否可读写等信息 , 和ls相比，ll会列出该文件下的所有文件信息，包括隐藏的文件，而ls -l只列出显式文件，说明这两个命令还是不等同的！

7. git cherry-pick 命令介绍.

	简单来说：git cherry-pick用于把另一个本地分支的commit修改应用到当前分支。

	> 应用中,当开发在master分支时，而服务器运行的代码在server分支。这样当需要发布新版本时，需要首先到服务器端代码部分，执行以下命令，来使master分支代码到server分支，同时更新服务器端代码到最新。
	<code>
	# /bin/sh
	git checkout master
	git pull
	git checkout server
	git cherry-pick master
	</code>

8. 建立一个新的代码库

	``` $ sudo git init --bare sample.git ```

	添加新的用户,需要在.ssh/authorized_keys中添加用户的id_rsa.pub文件。

9. go语言部分主要是hot部分。
	42.62.2.62上的go服务分析。
	+ 使用go的beego框架，redis框架，和beego依赖的框架如mysql等，已经成功在本地运行。
	+ go语言使用的mysql是42.62.2.62的数据库，配置文件中直接配置的是本地，端口是默认的3306。
	用户名密码有两个，一个是空密码的root，一个是weico3密码是ef86157.<br/>
	go链接数据库的配置是一行 ```weico3:ef86157@tcp(127.0.0.1:3306)/weico3_dev?charset=utf8```
	+ 42.62.2.62在DnsPod上映射的域名是weico3，二级域名是weico.cc。端口是go配置文件app.conf中的8080，
	但是这部分并不能直接通过ip+端口来进行访问，在服务器42.62.2.62上，是通过nginx来进行分发处理的。
	+ 查看nginx的配置文件（配置文件位置通过命令 nginx -t 来找到）。配置文件nginx.conf中引用了ttt.conf,
	而ttt.conf文件开头的
	```
	upstream ttt{
        server 127.0.0.1:8080;
	}
	```
	定义了一个负载均衡。
	在server部分又监听80端口，定义了域名访问weico3.weico.cc,将所有请求```proxy_pass http://ttt;```转给了ttt，完成了到go服务的请求
	+ 个人猜测：数据库部分,go语言部分代码仅仅提供了对外的api，从数据库中读取数据；数据库的写入部分是weico_admin_v3部分来完成的。

10. 添加ssh到服务器
	
	> 实际使用的时候，其实就是将本地的id_rsa.pub添加到服务器的 authorized_keys 中
	<pre>
	# ssh-keygen -t rsa (连续三次回车,即在本地生成了公钥和私钥,不设置密码)
	# ssh root@172.24.253.2 "mkdir .ssh;chmod 0700 .ssh" (需要输入密码)
	# scp ~/.ssh/id_rsa.pub root@172.24.253.2:.ssh/id_rsa.pub (需要输入密码)
	然后在B上的命令:
	# touch /root/.ssh/authorized_keys (如果已经存在这个文件, 跳过这条)
	# cat /root/.ssh/id_rsa.pub >> /root/.ssh/authorized_keys (将id_rsa.pub的内容追加到 authorized_keys 中)
	回到A机器:
	# ssh root@172.24.253.2 (因为没有设置私钥密码, 所以不需要密码, 登录成功) </pre>
<pre>

账号：

1、监控宝
http://www.jiankongbao.com

weicolomo@gmail.com     weico+lomo  -> weico+lomo_
canon@3ciyuan.com     xinciyuan123【已弃用】
dev@weico.com       kUBZiiwEh3Bie8   -> kUBZiiwEh3Bie8_

2、snmp v3（所有机器）
weico     8899//==we

3、UCloud
dev@eicodesign.com
dev@5YxbNTdbMP  -> dev@5YxbNTdbMP_

4、DNSPod
info@eicodesign.com  // weico.com weico.net
9ol.=[;.  		-> 9ol.=[;._
weico@eicodesign.com // weico.cc weicopl.us
9ol.=[;. 		-> 9ol.=[;._
likan@eicodesign.com
9ol.=[;. 		-> 9ol.=[;.=

5、花生壳
weicodev/1q2w3e4r  -> 1q2w3e4r_

6 Linode    --- 
用户名: 1982870, 密码: 1982870123a
ssh root@106.185.52.238 -p 1010
Cec+Pav>kelv>iD%

7 Jenkins
http://weicodev.wicp.net:9090/
http://192.168.1.80:8080/
username: weico
password: fU$ij+Mod:aD]jaR

8 weico.com邮箱管理帐号
ym.163.com
admin@weico.com / OLuJRaDe51qQ  //error password

9. WeicoBUG跟踪系统

http://192.168.1.80/bugzilla/ （内网）
http://weicodev.wicp.net:8080/bugzilla/
dev@weico.com
1123581321



联系方式：

800083103     广东锐讯网络有限公司
1171533501     森华 销售 看丹桥-梁爽
938060261      森华 看丹桥
712957909     美国机房
2880269152     锐讯-周鸿升
609509091     李侃推荐 服务器-DELL-聂伟


服务器列表：

ssh deployer@42.62.2.50 - 
主要是weico的好友数据库，以及postgres地理位置数据，基本不用特别维护

ssh deployer@42.62.2.51 - 
1. 新的小游戏平台weicogame服务（包括后端API以及前端服务）
2. 方糖相机资源管理API服务
3. 方糖相机，下载跳转jumper服务
4. 方糖相机用户数据上报服务

ssh deployer@42.62.2.58  - 
1. 微可拍API /home/deployer/developer/weicoplus (编译后的)
2. Pinco服务
3. Pinco的Tag搜索服务
4. weico api服务

ssh deployer@42.62.2.59  - 
1. 微可拍粉丝数据SSDB

ssh deployer@42.62.2.60 -
1. 文件存储服务器

ssh deployer@42.62.2.61 -
1. 微可拍好友动态数据SSDB

ssh deployer@42.62.2.62 -
1. weico3 数据库

ssh weico3@42.62.2.62
1. weico3 服务 (ssh weico3@42.62.2.62)
     文件位置：/mnt/tfs7/weico3/go/src/com.weico/weico3 
     项目地址：git@weicodev.vicp.cc:/mnt/data/repo/weico_server_go_v3.git

ssh deployer@118.26.233.56 -
1. 微可拍老接口 （/home/deployer/project/weicolomo/）
2. 微可拍用户接口 （/home/deployer/project/profile/）
3. 微可拍接口 （/home/deployer/androidtest/weicolomo）
4. 微可拍辅库
5. Redis服务（weico_user_q定时删除）

ssh deployer@118.26.233.57
1. 微可拍主库

ssh deployer@118.26.225.125
1. weico.com/eicodesign.com等官网
2. 数据库访问 mysql -h 127.0.0.1 -uroot
3. eico的用户管理erp http://118.26.225.125:8087/

ssh deployer@119.147.137.69
ssh deployer@119.90.40.250

ssh ubuntu@120.132.54.51
1. weiconote国内服务器
su: note@weico2014++

ssh deployer@106.185.52.238 -p 1010
1. weiconote海外服务器地址（oversea, japan, korean）
     项目路径 /home/deployer/developer/workspace/src/com.weico
2. redis服務 redis-cli
3. mysql服务 mysql -uroot -proot




常见错误处理：
1. 服务器硬盘快满了
cd /usr/local/nginx/logs/  或者
cd /usr/local/tengine/logs
cp /dev/null 比较大的日志.log

2. 118.26.233.56 的mysql主从备份出错
https://alexzeng.wordpress.com/2013/10/17/how-to-fix-mysql-slave-after-relay-log-corrupted/

mysql -uroot -proot

show slave status\G;

stop slave;

CHANGE MASTER TO MASTER_LOG_FILE='mysql-bin.000737', MASTER_LOG_POS=625032890;

start slave;

3. weico+服务无法访问，先查看42.62.2.58上的10010端口的revel 服务是否正常运行，没有则运行 revel run com.weico/weicoplus prod 10010
如果服务正常运行，则查看是不是日志满了

4.  42.62.2.51服务器添加服务端口
     firewall-cmd --permanent --zone=public --add-port=10020/tcp
     firewall-cmd --reload

</pre>