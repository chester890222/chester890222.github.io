---
layout: post
title: Python学习笔记
author: Chen Chen
description: "Python的学习笔记和注意事项"
header-img: "img/python-bg.jpg"
tags: ["Python"]
---
### 注意事项
1. Python中区分大小写！
2. Python用空格控制代码结构，不像C++等用括号。
3. “=”对象引用与其他语言有不同！

### 函数



### Class


#### private 
在变量前加`__`，外部不能直接访问。类似C++中的private/protected

有些时候，看到以一个下划线开头的实例变量名，比如_name，这样的实例变量外部是可以访问的，但是，按照约定俗成的规定，当你看到这样的变量时，意思就是，“虽然我可以被访问，但是，请把我视为私有变量，不要随意访问”。

#### 获取对象信息

- isinstance()
- dir()查看对象的所有属性和方法
- getattr()，setattr()，hasattr()可以用来操作对象的状态。

#### 动态绑定
支持在程序中定义methods，绑定指定实例或者class，相比其他语言只能在class中定义更加灵活

#### 定制类

##### __init__(self,...)
创建class实例时自动调用的初始化定义。

##### __slots__
在class定义中定义的变量，用来限制实例中能添加的属性。

>在继承的子类实例中不受影响，需要另外定义。

##### __str__

{% highlight python %}
>>> class Student(object):
     def __init__(self, name):
         self.name = name
     def __str__(self):
         return 'Student object (name: %s)' % self.name

>>> print(Student('Michael'))
Student object (name: Michael)
{% endhighlight %}

##### __repr__()
调试中使用的，功能与__str__类似。
 
{% highlight python %}
class Student(object):
    def __init__(self, name):
        self.name = name
    def __str__(self):
        return 'Student object (name=%s)' % self.name
    __repr__ = __str__
{% endhighlight %}

##### __iter__()
如果一个类想被用于循环，就需要用到__iter__，不停调用__next__()，直到返回StopIteration()。
 
{% highlight python %}
class Fib(object):
    def __init__(self):
        self.a, self.b = 0, 1 # 初始化两个计数器a，b

    def __iter__(self):
        return self # 实例本身就是迭代对象，故返回自己

    def __next__(self):
        self.a, self.b = self.b, self.a + self.b # 计算下一个值
        if self.a > 100000: # 退出循环的条件
            raise StopIteration();
        return self.a # 返回下一个值
{% endhighlight %}

##### 其他
- __getitem__()
- __getattr__()
- __call__()


#### @property
通过@property把方法变成属性调用，通过setter和getter定义限制。
如果只定义getter，不定义setter，则是一个只读属性。