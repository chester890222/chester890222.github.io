---
layout: post
title: MySQL基础(一)
author: Chen Chen
description: "MySQL数据库可以分为definition, manipulation和control三个部分，这篇文章主要是关于DDL和DCL的学习笔记，"
header-img: "img/Datacenter-bg.jpg"
tags: ["MySQL"]
---

SQL语句可以分为3类:
- Data Definition Languages (DDL)
- Data Manipulation Language (DML)
- Data Control Language (DCL)

## 启动MySQL

#### 1. 启动/结束MySQL服务
(在cmd中启动，需要电脑管理员权限，否则报错5) 

{% highlight console %}
net start/stop MySQL_name
{% endhighlight %}

#### 2. 连接到MySQL服务器   
在cmd中运行以下代码：

{% highlight console %}
mysql -uroot -p   

#访问远程数据库
mysql -h 10.108.108.253 -u chenchensh -p
{% endhighlight %}
-u 指定数据库用户   
-p 表示需要密码
-h 表示要连接的数据库位置，本地可不加或者用localhost

#### 3. 批量执行SQL语句
假设SQL语句保存在mysql.sql文件中。
 
{% highlight console %}
source mysql.sql
{% endhighlight %}

## DDL

DDL用来控制表的定义、结构、索引

### 创建/删除数据库   

通过create命令创建数据库。

{% highlight console %}
create database db_name;
{% endhighlight %}

drop命令用于删除数据库。 
 
{% highlight console %}
mysql> drop database db_name;
{% endhighlight %}

删除一个已经确定存在的数据库：
 
{% highlight console %}
mysql> drop database drop_database;
Query OK, 0 rows affected (0.00 sec)
{% endhighlight %}

删除一个不确定存在的数据库：
 
{% highlight console %}
mysql> drop database drop_database;
ERROR 1008 (HY000): Can't drop database 'drop_database'; database doesn't exist

mysql> drop database if exists drop_database;
Query OK, 0 rows affected, 1 warning (0.00 sec)
//产生一个警告说明此数据库不存在

mysql> create database drop_database;  
Query OK, 1 row affected (0.00 sec)
// 创建一个数据库

mysql> drop database if exists drop_database;  
Query OK, 0 rows affected (0.00 sec)
// if exists 判断数据库是否存在，不存在也不产生错误
{% endhighlight %}

### 创建表（删除drop)  
 
 {% highlight console %}
create table tbname(
    column_name1 data_type1 constraints,   
    column_name2 data_type2 constraints,   
    ……   
) 
 {% endhighlight %}
 
### 修改表

#### 1. 修改表类型
 
{% highlight console %}
alter table tbname modify [column] column_definition [first|after col_name]
{% endhighlight %}

#### 2. 增加表字段   
 
{% highlight console %}
alter table tbname add [column] column_definition [first|after col_name]
{% endhighlight %}

#### 3. 删除表字段   
 
{% highlight console %}
alter table tbname drop [column] col_name
{% endhighlight %}

#### 4. 修改字段名   
 
{% highlight console %}
alter table tbname change [column] old_name col_definition [first|after col_name]
{% endhighlight %}

#### 5. 修改字段位置   
first和after用来定义位置，add默认在表最后，change/modify不改变位置。
`注意：change first after column属于MySQL，其他数据库可能不通用`

#### 6. 更改表名字   
 
{% highlight console %}
alter table tbname rename [to] new_tbname;
{% endhighlight %}


## DCL

DCL是用来管理系统中对象权限的，比如创建用户、用户授权等

> 例子   
grant select, insert on test1.* to ‘zl’@’localhost’ identified by ‘123’;   
创建一个数据库用户zl，对test1数据库中所有表的select/insert权限，密码123   
之后可以用mysql -uzl -p123登录，并操作zl数据库   
revoke insert on test1.* from ‘zl’@’localhost’;   
取消了zl的insert权限。

可以用? contents查看帮助

### 创建用户
 
{% highlight console %}
CREATE USER 'username'@'host' IDENTIFIED BY 'password'; 
{% endhighlight %}
*username* - 你将创建的用户名，host – 指定该用户在哪个主机上可以登陆，如果是本地用户可用localhost，如果想让该用户可以从任意远程主机登陆，可以使用通配符%。
*password* - 该用户的登陆密码，密码可以为空，如果为空则该用户可以不需要密码登陆服务器。

{% highlight console %}
CREATE USER 'dog'@'localhost' IDENTIFIED BY '123456'; 
CREATE USER 'pig'@'192.168.1.101_' IDENDIFIED BY '123456'; 
CREATE USER 'pig'@'%' IDENTIFIED BY '123456'; 
CREATE USER 'pig'@'%' IDENTIFIED BY ''; 
CREATE USER 'pig'@'%';
{% endhighlight %}

### 授权
 
{% highlight console %}
GRANT privileges ON databasename.tablename TO 'username'@'host' 
{% endhighlight %}
*privileges* - 用户的操作权限，如SELECT , INSERT, UPDATE 等（详细列表见文章最后）。如果要授予所有的权限则使用ALL。
*databasename* - 数据库名
*tablename* - 表名，如果要授予该用户对所有数据库和表的相应操作权限则可用\*.*表示.
 
{% highlight console %}
GRANT SELECT, INSERT ON test.user TO ‘pig’@’%’; 
GRANT ALL ON . TO ‘pig’@’%’; 
{% endhighlight %}

`注意：`用以上命令授权的用户不能给其它用户授权,如果想让该用户可以授权,用以下命令: 
 
{% highlight console %}
GRANT prvileges ON databasename.tablename TO 'username'@'host' WITH GRANT OPTION；
{% endhighlight %}

### 设置与更改用户密码
 
{% highlight console %}
SET PASSWORD FOR 'username'@'host' = PASSWORD('newpassword'); 
{% endhighlight %}

对于当前登陆的用户，则可以用
 
{% highlight console %}
SET PASSWORD = PASSWORD("newpassword");
{% endhighlight %}

### 撤销用户权限
 
{% highlight console %}
REVOKE privilege ON databasename.tablename FROM 'username'@'host'; 
REVOKE SELECT ON *.* FROM 'pig'@'%';
{% endhighlight %}

`注意：`假如你在给用户’pig’@’%’授权的时候类似于这样，那在使用后面一条命令并不能撤销该用户对test数据库中user表的SELECT操作。
 
{% highlight console %}
GRANT SELECT ON test.user TO ‘pig’@’%’;

REVOKE SELECT ON . FROM ‘pig’@’%’;
{% endhighlight %}

相反，如果授权使用的是下面的命令，则之后那条命令也不能撤销该用户对test数据库中user表的 Select权限。
 
{% highlight console %}
GRANT SELECT ON . TO ‘pig’@’%’;
REVOKE SELECT ON test.user FROM ‘pig’@’%’;
{% endhighlight %}

具体信息可以用以下命令查看：
 
{% highlight console %}
SHOW GRANTS FOR ‘pig’@’%’; 
{% endhighlight %}


### 删除用户
 
{% highlight console %}
DROP USER ‘username’@'host’; 
{% endhighlight %}


>例子：一个典型的数据库建表, 建用户过程:
 
{% highlight console %}
#创建用于localhost连接的用户并指定密码 
mysql> create user 'pcom'@'localhost' identified by 'aaa7B2249'; 
Query OK, 0 rows affected (0.00 sec) 

#创建数据库 
mysql> create database pcom default character set utf8 collate utf8_bin; 
Query OK, 1 row affected (0.00 sec) 

#给本地用户授权, 这里不需要指定密码 
mysql> grant all on pcom.* to 'pcom'@'localhost'; 
Query OK, 0 rows affected (0.00 sec) 

#给其他IP地址下的用户授权, 注意: 这里必须指定密码, 否则就可以无密码访问 
mysql> grant all on pcom.* to 'pcom'@'192.168.0.0/255.255.0.0' identified by 'aaa7B2249'; 
Query OK, 0 rows affected (0.00 sec) 

#同理 
mysql> grant all on pcom.* to 'pcom'@'172.20.0.0/255.255.0.0' identified by 'aaa7B2249'; 
Query OK, 0 rows affected (0.00 sec) 

#刷新数据库权限
mysql> flush privileges;

{% endhighlight %}

## 附表：在MySQL中的操作权限
ALTER 
Allows use of ALTER TABLE.

ALTER ROUTINE 
Alters or drops stored routines.

CREATE 
Allows use of CREATE TABLE.

CREATE ROUTINE 
Creates stored routines.

CREATE TEMPORARY TABLE 
Allows use of CREATE TEMPORARY TABLE.

CREATE USER 
Allows use of CREATE USER, DROP USER, RENAME USER, and REVOKE ALL PRIVILEGES.

CREATE VIEW 
Allows use of CREATE VIEW.

DELETE 
Allows use of DELETE.

DROP 
Allows use of DROP TABLE.

EXECUTE 
Allows the user to run stored routines.

FILE 
Allows use of SELECT… INTO OUTFILE and LOAD DATA INFILE.

INDEX 
Allows use of CREATE INDEX and DROP INDEX.

INSERT 
Allows use of INSERT.

LOCK TABLES 
Allows use of LOCK TABLES on tables for which the user also has SELECT privileges.

PROCESS 
Allows use of SHOW FULL PROCESSLIST.

RELOAD 
Allows use of FLUSH.

REPLICATION 
Allows the user to ask where slave or master

CLIENT 
servers are.

REPLICATION SLAVE 
Needed for replication slaves.

SELECT 
Allows use of SELECT.

SHOW DATABASES 
Allows use of SHOW DATABASES.

SHOW VIEW 
Allows use of SHOW CREATE VIEW.

SHUTDOWN 
Allows use of mysqladmin shutdown.

SUPER 
Allows use of CHANGE MASTER, KILL, PURGE MASTER LOGS, and SET GLOBAL SQL statements. Allows mysqladmin debug command. Allows one extra connection to be made if maximum connections are reached.

UPDATE 
Allows use of UPDATE.

USAGE 
Allows connection without any specific privileges.

