---
layout: post
title: Python notes：Data Types
author: Chen Chen
description: "Python中常用的数据类型"
header-img: "img/python-bg.jpg"
tags: ["Python"]
---

## 常用数据类型

|     类型    |           例子           |
|-------------|--------------------------|
| int         | `-100`                   |
| float       | `3.1416`                 |
| long | `1000000000000L`|
| bool | `True, False`|
| string      | `'hello'`                |
| list        | `[1, 1.2, 'hello']`      |
| tuple | `('ring', 1000)`|
| set | `{1, 2, 3}`|
| dict        | `{'dogs': 5, 'pigs': 3}` |
| Numpy array | `array([1, 2, 3])`       |
| Pandas types| `DataFrame, Series`|


可以用type()来检测任何变量的类型。

### 1. Booleans
True(=1)、False(=0)


{% highlight python %}
>>>True + True 
2
{% endhighlight %}


### 2. Numbers

系统自动检测int、float，也可以通过int()，float()强制转换。
`int()是取整，不是四舍五入！`

当整数int超出范围时，Python会自动转换为long int。不过计算速度比int慢，在数字后以字母L结尾。

float精确到小数点后15位，print时会自动校准。

Python还支持复数，用j表示imaginary part

{% highlight python %}
>>>a = 1+2j
>>>type(a)
complex
{% endhighlight %}


### 3. Strings

' '和" "都可以定义string，可以做简单的运算。用3个引号'''   '''定义多行字符串。

{% highlight python %}
>>>('ch' + 'en') * 2
'chenchen'
{% endhighlight %}

不能通过索引修改，可以用str.replace('from', 'to')修改

### 4. Lists

用[ ]定义列表，也通过list[i]取值，`第一个i=0，倒数i=-1`。
 
- 用[ ]定义列表，也通过list[i]取值，`第一个i=0，倒数i=-1`。
 
- 插入新增项

{% highlight python %}
>>> a_list = ['a'] 
>>> a_list = a_list + [2.0, 3]    
>>> a_list                        
['a', 2.0, 3] 
>>> a_list.append(True)           
>>> a_list 
['a', 2.0, 3, True] 
>>> a_list.extend(['four', 'Ω'])  
>>> a_list 
['a', 2.0, 3, True, 'four', 'Ω'] 
>>> a_list.insert(0, 'Ω')         
>>> a_list 
['Ω', 'a', 2.0, 3, True, 'four', 'Ω'] 
#append和extend有区别,append里的list被当做一个element
>>> a_list.append(['x','y','z'])
>>> a_list
['Ω', 'a', 2.0, 3, True, 'four', 'Ω', ['x','y','z']] 

{% endhighlight %}

- 列表中检索
 
{% highlight python %}
>>> a_list = ['a', 'b', 'new', 'mpilgrim', 'new'] 
>>> a_list.count('new')       
2 
>>> 'new' in a_list         
True 
>>> 'c' in a_list 
False 
>>> a_list.index('mpilgrim')  
3 
>>> a_list.index('new')       
2
Traceback (innermost last): 
  File "<interactive input>", line 1, in ?ValueError: 
list.index(x): x not in list 
{% endhighlight %}




### 5. Tuple

用()定义元组，不可变。


#### 元组与列表的区别：
- 元组的生成速度快一个数量级
- 迭代速度更快
- 不可变，更“安全”
- 可以通过tuple(alist)或者list(atuple)相互转换。

### 6. Dictionaries

用{}定义字典，通过['key']来查询和插入value。

通过get('key')来获取value，如果key不存在，返回None， 不报错。

key-value存储，查找速度极快。
- 一个key只能对应一个value
- 不存在的key会报错，可用`in`判断或者get(key)。
- dict内部存放顺序与写入顺序无关！

### 7. Sets

集合是装有独特值的无序“袋子”。可以执行像联合、交集以及集合求差等标准集合运算。

- 可以通过set(alist)创建，也可以{1,2,3}定义。但空集合只能用set()，{}表示dict。
- add() 添加单个成员，但如果x已经在set里，则set不变。
- update()只能使用list/set参数，可多个参数同时添加。
- discard()和remove()删除值，如果参数不在set里， discard不操作，remove报错
- a.union(b)返回a、b的并集（a|b)
- a.intersection(b)返回a、b的交集（a&b）
- a.difference(b)返回a中出现、b中没用的（a-b）
- a.symmetric_difference(b)返回只在a或b出现的（a^b)
- a.issubset(b)返回a是不是b的子集(a<=b)
- a.issuperset(b)返回a是不是b的超集

#### Frozen set

用frozenset(list)创建，一旦创建就不可以改变。主要用作字典的key。


### 8. Enum


{% highlight python %}
from enum import Enum

Month = Enum('Month', ('Jan', 'Feb', 'Mar'))
{% endhighlight %}


## 索引与分片

通过[]来对有序序列索引，可正向（从0开始）、反向(-1开始)。

字典是没有顺序的（不按输入顺序排列）。

分片用法： `var[lower:upper:step]`

分片的范围包括lower，不包括upper！

## Mutable

数据类型分类：

|                            可变数据类型                            |                            不可变数据类型                             |
|--------------------------------------------------------------------|-----------------------------------------------------------------------|
| `list`, `dictionary`, `set`, `numpy array`, `user defined objects` | `integer`, `float`, `long`, `complex`, `string`, `tuple`, `frozenset` |
