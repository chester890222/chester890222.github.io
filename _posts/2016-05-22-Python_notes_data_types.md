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

#### 4. Bytes

#### 5. Lists
- 用[ ]定义列表，也通过list[i]取值，`第一个i=0，倒数i=-1`。
 
- 插入新增项