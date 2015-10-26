+++
date = "2015-10-26T15:34:15+08:00"
draft = true
title = "学习nginx的安装、配置"

+++


#### nginx的本地安装
	
1. 首先是要在本地安装一个nginx服务器，来看看最基本的nginx是神马样子的。安装方式并不复杂，而且nginx完全是通过c来编写的，依赖并不多，整个服务器才不到1Mb。

	> 特殊的，nginx是有很多第三方包的，而且实际调试使用时基本是必须的，如果一个一个安装显得过于繁琐，这里有一个配置好了各个常用插件的nginx的版本：openresty,下载下来进行安装即可。mac下需要首先安装```PCRE```，然后通过
	> <pre>
	$ ./configure \
             --with-cc-opt="-I/usr/local/include" \
             --with-ld-opt="-L/usr/local/lib" \ </pre>
    > 进行配置，然后通过```make && make install``` 进行安装

2. 本地运行环境是mac，需要安装一个perl的正则解析PCRE
	<pre>
	$ cd ~/Downloads
	$ tar xvzf pcre-8.5
	$ cd pcre-8.5
	$ sudo ./configure --prefix=/usr/local
	$ sudo make
	$ sudo make install	</pre>

3. 然后安装nginx
	<pre>
	$ cd ~/Downloads
	$ tar xvzf nginx-1.6.0.tar.gz
	$ cd nginx-1.6.0
	$ sudo ./configure --prefix=/usr/local/nginx --with-http_ssl_module --with-cc-opt="-Wno-deprecated-declarations"
	$ sudo make
	$ sudo make install	</pre>

4. 尝试启动nginx，
	<pre>
	$ cd /usr/local/nginx/sbin
	$ sudo ./nginx	</pre>
	然后访问```localhost```,如果页面显示```Welcome to nginx!```,则说明nginx安装成功并成功启动。

5. 停止nginx
	<pre>
	sudo ./nginx -s stop 	</pre>
	之后可以将nginx加入到path中去。

6. 如果nginx停止失败，可以通过kill命令来通知nginx停止
	+ 首先查看nginx运行的情况 ```ps -ef|grep nginx```
	+ 找到master进程，它的编号就是主进程号了，通过 ```sudo kill -QUIT 8339 将其杀掉```。

7. 修改了配置文件，首先校验配置文件是否正确
	<pre>
	nginx -t -c nginx.conf
	或者
	nginx -t </pre>

8. 不重启nginx，应用新的配置文件.（首先要使用步骤7校验配置文件）
	<pre>
	nginx -s reload </pre>


