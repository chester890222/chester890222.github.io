---
layout: post
title: Python笔记：基础
author: Chen Chen
description: "Python的基础使用笔记"
header-img: "img/python-bg.jpg"
tags: ["Python"]
---

## Python 赋值

python中的赋值机制与其他语言有所区别，使用时需要注意。
 
{% highlight python %}
>>>x = [1,2,3]
>>>y = x
>>>x[1] = 100
>>>y
[1,100,3]

>>>x=500
>>>y=x
>>>y
500
>>>y='foo'
>>>x
500
>>>y
foo


>>>x = [500, 501, 502]
>>>y = x
>>>print('x = ',x,' y= ',y)
x =  [500, 501, 502]  y=  [500, 501, 502]
>>>y[1] = 700
>>>print('x = ',x,' y= ',y)
x =  [500, 700, 502]  y=  [500, 700, 502]
>>>y = [100,200]
>>>print('x = ',x,' y= ',y)
x =  [500, 700, 502]  y=  [100, 200]
{% endhighlight %}


## 条件控制
在C++等语言中用{ }定义，Python中用`:`以及缩进控制代码块。

{% highlight python %}
if condtion:
     statement
elif condition2:
    statement2
else:
    statement3
{% endhighlight %}

## 实用功能

### 列表推导式

{% highlight python %}
#列表推导式，也适用于集合、字典
values = [10,21,3,5,12]
squares = [x**2 for x in values if x<=10]
square_set = {x**2 for x in values if x<=10}
square_dict = {x: x**2 for x in values if x<=10}
{% endhighlight %}

### 函数

用def开头，不需要指定返回值，return语句返回值，否则默认空值None。
变量也不需要指定类型

{% highlight python %}
def func_name(pram1, param2 = default_value, ....):
  return x
{% endhighlight %}

### 文档注释
用三重引号表示多行字符串。使用是可以通过$times.__doc__$来获取。

{% highlight python %}
def times(a,b):
  '''a multiplies b.

  key arguments:
  a -- number
  b -- number

  returns: number
  '''
    a*b

{% endhighlight %}
 
{% highlight python %}
#接受不定参数
def add(x, *args):
    total = x
    for arg in args:
        total += arg
    return total

>>> add(1,2,3,5,)
11

#使用包含关键词的不定参数
def add(x, **args):
    total = x
    for arg, value in args.items():
        print('adding ', arg)
        total += value
    return total

>>>add(1,x=1,y=3,z=100)
105

{% endhighlight %}

将函数fun应用到seq序列上的每个元素
map(fun, seq)


### 模块和包

可以通过`import module`，或者通过`from module import part`调用全部或部分模块

#### __name__属性

一个.py文件，可能是模块，也可以当作脚本使用（测试用）。
 
{% highlight python %}
#文件名称 mymodule.py
PI = 3.1416

def sum(lst):
    """ Sum the values in a list
    """
    tot = 0
    for value in lst:
        tot = tot + value
    return tot

def add(x, y):
    " Add two values."
    a = x + y
    return a

def test():
    w = [0,1,2,3]
    assert(sum(w) == 6)
    print 'test passed.'
    
if __name__ == '__main__':
    test()
{% endhighlight %}

这样的文件既可以当模块调用，也可以当脚本运行。
 
{% highlight python %}
>>>run mymodule.py
test passed
>>>import mymodule as my
>>>my.PI
3.1416
{% endhighlight %}

#### 包（package)

假设我们有这样的一个文件夹：

foo/
- `__init__.py` 
- `bar.py` (defines func)
- `baz.py` (defines zap)

这意味着 foo 是一个package，可以这样导入其中的内容：

{% highlight python %}
from foo.bar import func
from foo.baz import zap
{% endhighlight %}

`bar` 和 `baz` 都是 `foo` 文件夹下的 `.py` 文件。

导入包要求：
- 文件夹 `foo` 在**Python**的搜索路径中
- `__init__.py` 表示 `foo` 是一个包，它可以是个空文件。


### 异常exception
通过try....except来避免系统报错

 
{% highlight python %}
import math

while True:
    try:
        text = raw_input('> ')
        if text[0] == 'q':
            break
        x = float(text)
        y = 1 / math.log10(x)
        print "1 / log10({0}) = {1}".format(x, y)
    except ValueError as exc:
        if exc.message == "math domain error":
            print "the value must be greater than 0"
        else:
            print "could not convert '%s' to float" % text
    except ZeroDivisionError:
        print "the value must not be 1"
    except Exception as exc:
        print "unexpected error:", exc.message
{% endhighlight %}


