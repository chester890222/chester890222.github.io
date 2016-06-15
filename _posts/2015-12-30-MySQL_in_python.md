---
layout: post
title: Python中使用MySQL
author: Chen Chen
description: "在python中使用MySQL数据库的基础笔记"
header-img: "img/Datacenter-bg.jpg"
tags: ["MySQL","Python"]
---

MySQL Connector/Python 是MySQL官方提供的Python连接MySQL数据库的驱动程序。相较于MySQLdb模块来说，其支持python3，而MySQLdb目前只支持到python2.7版本。pandas也支持数据库的读写。

## 安装
可以直接使用下列命令安装：
 
{% highlight console %}
pip install mysql-connector-python
{% endhighlight %}
也可以从官方下载mysql-connector-python的安装包使用。

## 使用

### 1. 建立连接
 

{% highlight python %}
import mysql.connector
config={'host':'127.0.0.1',#默认127.0.0.1
        'user':'root',
        'password':'123456',
        'port':3306 ,#默认即为3306
        'database':'test',
        'charset':'utf8'#默认即为utf8
        }
try:
  cnn=mysql.connector.connect(**config)
except mysql.connector.Error as e:
  print('connect fails!{}'.format(e))
{% endhighlight %}

如果要使用连接池功能可以多指定一个参数：pool_size或者pool_name：
 
{% highlight python %}
conn=mysql.connector.connect(host="127.0.0.1",user="test",password="1234",pool_size=10)
{% endhighlight %}
该命令会从连接池中分配一个连接使用。

### 2. 执行语句
 
{% highlight python %}
cur=conn.cursor()
cur.execute("show databases")
cur.fetchall()
{% endhighlight %}


### 3. 创建表格

下面我们根据上面新建的一个数据库连接创建一张名为student的表：
 
{% highlight python %}
sql_create_table='CREATE TABLE `student` \
	(`id` int(10) NOT NULL AUTO_INCREMENT,\
	`name` varchar(10) DEFAULT NULL,\
	`age` int(3) DEFAULT NULL,\
	PRIMARY KEY (`id`)) \
	ENGINE=MyISAM DEFAULT CHARSET=utf8'

cursor=cnn.cursor()
try:
  cursor.execute(sql_create_table)
except mysql.connector.Error as e:
  print('create table orange fails!{}'.format(e)) 
{% endhighlight %}


###4. 插入数据
插入数据的语法上和MySQLdb上基本上是一样的：
 
{% highlight python %}
cursor=cnn.cursor()
try:
  #第一种：直接字符串插入方式
  sql_insert1="insert into student (name, age) values ('orange', 20)"
  cursor.execute(sql_insert1)
  #第二种：元组连接插入方式
  sql_insert2="insert into student (name, age) values (%s, %s)"
  #此处的%s为占位符，而不是格式化字符串，所以age用%s
  data=('shiki',25)
  cursor.execute(sql_insert2,data)
  #第三种：字典连接插入方式
  sql_insert3="insert into student (name, age) values (%(name)s, %(age)s)"
  data={'name':'mumu','age':30}
  cursor.execute(sql_insert3,data)
  #如果数据库引擎为Innodb，执行完成后需执行cnn.commit()进行事务提交
except mysql.connector.Error as e:
  print('insert datas error!{}'.format(e))
finally:
  cursor.close()
  cnn.close()
{% endhighlight %}


同样，MySQL Connector也支持多次插入，同样其使用的也是cursor.executemany，示例如下：
 
{% highlight python %}
stmt='insert into student (name, age) values (%s,%s)'
data=[
     ('Lucy',21),
     ('Tom',22),
     ('Lily',21)]
cursor.executemany(stmt,data)
{% endhighlight %}


###5. 查询操作
 
{% highlight python %}
cursor=cnn.cursor()
try:
  sql_query='select id,name from student where  age > %s'
  cursor.execute(sql_query,(21,))
  for id,name in cursor:
    print('%s＼'s age is older than 21,and her/his id is %d'%(name,id))
except mysql.connector.Error as e:
  print('query error!{}'.format(e))
finally:
  cursor.close()
  cnn.close()
{% endhighlight %}


###6. 删除操作
 
{% highlight python %}
cursor=cnn.cursor()
try:
  sql_delete='delete from student where name = %(name)s and age < %(age)s'
  data={'name':'orange','age':24}
  cursor.execute(sql_delete,data)
except mysql.connector.Error as e:
  print('delete error!{}'.format(e))
finally:
  cursor.close()
  cnn.close()
{% endhighlight %}


### 7. 关闭连接
 
{% highlight python %}
conn.close()
{% endhighlight %}
如果conn是直接新建的连接则它会被关闭，如果是从线程池中分配一个连接则会被归还给连接池

### 8. 关于游标

mysql-python支持两种类型的游标：cursor和sscursor 
根据文档介绍sscursor是server-side游标，但实际不是的，mysql本身不支持server-side游标。sscursor实际是一个non_buffered cursor。而cursor是一个buffered cursor。

mysql-connector-python正好相反，它默认使用的是non_buffered cursor，创建buffered cursor时需要显示指定参数。

 
{% highlight python %}
#创建non_buffered cursor:
cur=conn.cursor()

#创建buffered cursor
cur=conn.cursor(buffered=True)
{% endhighlight %}

buffrered cursor与non_buffered cursor的区别见[官方说明](http://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursorbuffered.html "http://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursorbuffered.html")

mysql-connector-python正在模拟实现server-side cursor，实现办法是将查询结果集存放在server端的一个临时表中，客户端调用 fetch方法时从临时表中返回结果。[Read more](http://geert.vanderkelen.org/simulating-server-side-cursors-with-mysql-connectorpython/ "http://geert.vanderkelen.org/simulating-server-side-cursors-with-mysql-connectorpython/")
