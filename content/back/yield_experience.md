+++
date = "2015-10-26T15:33:12+08:00"
draft = true
title = "后台经验"

+++


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

11. 部署weico3到服务器端，进行相关的处理
	<pre>
	//linux平台编译
	GOOS=linux GOARCH=amd64 go build

	//上传新的包
	scp -i ~/workspace/weico_rsa weico3 root@42.62.2.62:/mnt/tfs7/weico3/go/src/com.weico/weico3_new

	//杀掉weico3进程
	kill -s 9 $1
	//重命名
	mv weico3 weico3_old
	mv weico3_new weico3
	chmod 777 weico3
	//切换用户
	su weico3
	//不挂起启动
	nohup ./weico3 &
	</pre>

12. 清理redis指定key的缓存
	
	删除所有weico开头的缓存 ```redis-cli keys "weico*" | xargs redis-cli del```

13. 得到服务器的对外端口

	是否开启了21端口: ``` 	netstat -ntpl|grep 21	 ```

	结果显示
	```tcp6       0      0 :::21                   :::*                    LISTEN      27660/vsftpd```

14. git服务器地址迁移。
	
	迁移git服务器其实很简单，在新的服务器上初始化新的git包，或者直接将原来的git包复制过来即可。在客户端，需要修改一下remote地址

	新的服务器地址是``` dev.weico.com ```，代码放在android用户目录下，最后拼装成新的git地址是:

	``` git clone root@dev.weico.com:/home/android/WeicoTV.git ```

	在本地项目目录下，首先移除原来的remote地址:

	``` git remote remove origin ```

	然后添加新的remote地址:

	``` git remote add origin root@dev.weico.com:/home/android/WeicoTV.git ```

	重新push一下即可，这样会将本地所有的修改提交到新的服务器上。（服务器端采用复制git包或者重新初始化git包的方式，都可以将代码更新成最新的）



	