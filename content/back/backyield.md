+++
date = "2015-10-26T15:40:55+08:00"
draft = true
title = "后端交接文档"

+++


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

更新为
	qihuan@weico.com
	weico2014++

4、DNSPod
info@eicodesign.com  // weico.com weico.net
9ol.=[;.  		-> 9ol.=[;._
weico@eicodesign.com // weico.cc weicopl.us
9ol.=[;. 		-> 9ol.=[;._
likan@eicodesign.com
9ol.=[;. 		-> 9ol.=[;.=

5、花生壳
weicodev/1q2w3e4r  -> 1q2w3e4r_
				   -> 1q2w3e4r

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
5. meShot资源管理 ResourceManager

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
 ip更新为 120.132.56.18
 sudo weico2014++
 sudo -i 切换为root用户

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


5. 海蜘蛛路由器密码
	路由器管理地址：http://10.0.1.254:880/
	管理页面登陆账号:admin - r3i2ojS_dcjir4f
	路由器拨号账号：
		1. 010010017063 - 731792
		2. 010999009413  - 暂未重置

</pre>