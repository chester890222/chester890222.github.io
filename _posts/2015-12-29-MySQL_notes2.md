---
layout: post
title: MySQL基础(二)
author: Chen Chen
description: "MySQL数据库可以分为definition, manipulation和control三个部分，这篇文章主要是关于DML数据查询及修改的学习笔记，"
header-img: "img/Datacenter-bg.jpg"
tags: ["MySQL"]
---

SQL语句可以分为3类:
- Data Definition Languages (DDL)
- Data Manipulation Language (DML)
- Data Control Language (DCL)

DDL和DCL的基础笔记在[这里]({% post_url 2015-12-29-MySQL_notes1 %} "{% post_url 2015-12-29-MySQL_notes1 %}")。

### DML

Data Manipulation Language(DML)是对数据库中的表记录的操作。

#### 1. 插入记录   

{% highlight console %}
insert into tbname(field1,field2, …) values(value1, value2, …);
{% endhighlight %}

也可以insert into tbname values(….), (….), …. 插入多个记录。

#### 2. 更新记录   

{% highlight console %}
update tbname set field1=value1, …, fieldn=valuen [where condition]   
{% endhighlight %}

可以同时更新多个表中的数据。多用于根据一个表的字段来动态更新另外一个表的字段。

> 例子：   
update emp1 a, dept b set a.sal=a.sal*b.deptno, b.deptname=a.ename where a.deptno=b.deptno;

#### 3. 删除记录   

{% highlight console %}
delete from tbname [where condition]   
delete t1, t2, … from t1, t1, … [where condition]   
{% endhighlight %}

`如果不加where condition，则会把表的所有记录删除！`

> 例子：   
delete a,b from emp1 a, dept b where a.deptno=b.deptno and a.deptno=3;

#### 4. 查询记录

* select * from tbname [where condition]   
*表示所有字段，也可用,分隔需要的字段。
    * select distinct …..去掉重复的记录
    * select * from tbname [where condition] [order by field1 [desc\asc], field2 [desc\asc], …..]来排序，desc、asc是降序和升序   
对field1,2依次排序，如果只有field1，则相同的field1内的记录无序排列。
    * select …. [limit offset_start, row_count]来显示一部分记录   
`limit 和order by常一起用于分页显示`

#### 5. 聚合   
select [field1, field2, …] fun_name   
from tbname   
[where condition] [group by field1, field2, …. [with rollup]]   
[having condition]   
fun_name表示要做的聚合操作，如sum, count, max, min   
group by要聚合的字段， with rollup表明是否对分类聚合的结果进行再汇总   
having表示对分类后的结果再进行条件过滤（where是聚合前的筛选，having是之后）

#### 6. 表连接   
分为内连接和外连接，内连接仅选出两张表中相互匹配的记录，外连接会选出其他不匹配的记录。   
外连接还分为左连接和右连接（包含左/右表中的所有记录，即使另一边没有匹配的记录）

> 例子(内连接)   
select ename,deptname from emp1, dept where emp1.deptno=dept.deptno

#### 7. 子查询   
select * from tbname where condition ( in/ not in / = / != / exists / not exists)

#### 8. 记录联合   
select * from tb1   
union/union all   
select * from tb2   
….   
union/union all   
select * from tbm;   
union all是把结果集直接合并在一起，union是将union all的结果进行一次distinct。


#### 9. 通配符

% 可以匹配任意字符   
_ 能且只能匹配一个字符

#### 10. 拼接字段

concat()

> 例子   
SELECT Concat(vend_name, ’ (‘, vend_country, ‘)’ )   
FROM vendors   
ORDER BY vend_name；   
显示name(location)

可以用Trim()、RTrim()或者LTrim()去掉左右的空格
