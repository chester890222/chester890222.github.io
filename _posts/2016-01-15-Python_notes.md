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

### 数据类型

可以用type()来检测任何变量的类型。

常用的有以下几种：

#### 1. Booleans
True(=1)、False(=0)
>True + True = 2

#### 2. Numbers
- 自动检测int、float，也可以通过int()，float()强制转换。
`int()是取整，不是四舍五入！`
- float精确到小数点后15位
- 整数任意大小

#### 3. Strings

#### 4. Bytes

#### 5. Lists
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

#### 6. Tuples
元组是不可变的列表。一旦创建之后，用任何方法都不可以修改。用( )定义，tuple[i]取值。
- 元组速度比列表快
- 更安全
- 可以通过tuple(alist)或者list(atuple)相互转换。

#### 7. Sets
集合是装有独特值的无序“袋子”。可以执行像联合、交集以及集合求差等标准集合运算。  
- 可以通过set(alist)转换。
- add() 添加单个成员，但如果x已经在set里，则set不变。
- update()只能使用list/set参数，可多个参数同时添加。
- discard()和remove()删除值，如果参数不在set里， discard不操作，remove报错
- a.union(b)返回a、b的并集
- a.intersection(b)返回a、b的交集
- a.difference(b)返回a中出现、b中没用的
- a.symmetric_difference(b)返回只在a或b出现的
- a.issubset(b)返回a是不是b的子集
- a.issuperset(b)返回a是不是b的超集

#### 8. Dict
key-value存储，查找速度极快。
- 一个key只能对应一个value
- 不存在的key会报错，可用`in`判断或者get(key)。
- dict内部存放顺序与写入顺序无关！

#### 9. Enum

 
{% highlight python %}
from enum import Enum

Month = Enum('Month', ('Jan', 'Feb', 'Mar'))
{% endhighlight %}



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