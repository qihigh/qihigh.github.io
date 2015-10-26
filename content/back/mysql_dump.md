+++
date = "2015-10-26T15:33:46+08:00"
draft = true
title = "Mysql导出表结构及表数据 mysqldump用法"

+++

#### 命令行下具体用法如下： 

> mysqldump -u用戶名 -p密码 -d 数据库名 表名 脚本名;

1. 导出数据库為dbname的表结构（其中用戶名為root,密码為dbpasswd,生成的脚本名為db.sql）

	```mysqldump -uroot -pdbpasswd -d dbname >db.sql;```

2. 导出数据库為dbname某张表(test)结构

	```mysqldump -uroot -pdbpasswd -d dbname test>db.sql;```

3. 导出数据库為dbname所有表结构及表數據（不加-d）

	```mysqldump -uroot -pdbpasswd  dbname >db.sql;```

4. 导出数据库為dbname某张表(test)结构及表數據（不加-d）

	```mysqldump -uroot -pdbpasswd dbname test>db.sql;```


#### 其他数据库相关
	查看MySQL端口号：```show global variables like 'port';  ```