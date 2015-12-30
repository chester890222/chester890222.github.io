---
layout: post
title: Python中使用MySQL
author: Chen Chen
description: "在python中使用mysql数据库的教程"
header-img: "img/Datacenter-bg.jpg"
tags: ["MySQL","Python"]
---


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
conn=mysql.connector.connect(host="127.0.0.1",user="test",password="1234")
{% endhighlight %}
该命令会返回一个mysql连接。
如果要使用连接池功能可以多指定一个参数：pool_size或者pool_name：
 
{% highlight python %}
conn=mysql.connector.connect(host="127.0.0.1",user="test",password="1234",pool_size=10)
{% endhighlight %}
该命令会从连接池中分配一个连接使用。

### 2. 执行
 
{% highlight python %}
cur=conn.cursor()
cur.execute("show databases")
cur.fetchall()
{% endhighlight %}

### 3. 关闭连接
 
{% highlight python %}
conn.close()
{% endhighlight %}
如果conn是直接新建的连接则它会被关闭，如果是从线程池中分配一个连接则会被归还给连接池

### 4. 关于游标

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