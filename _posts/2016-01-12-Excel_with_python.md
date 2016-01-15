---
layout: post
title: 用Python操作Excel文件
author: Chen Chen
description: "目前子账户系统无法正常使用的情况下，不得不把每天自己的交易记录导出，计算持仓、净值、盈亏等等。目前用的是excel表格记录，想把每天重复的活交给python，需要用到python读取、修改excel文件。"
header-img: "img/python-bg.jpg"
tags: ["Excel","Python"]
---

由于证监会的新规定，目前子账户系统无法正常使用的情况下，不得不把每天自己的交易记录导出，计算持仓、净值、盈亏等等。目前用的是excel表格记录，想把每天重复的活交给python，需要用到python读取、修改excel文件。

在实现自动计算后，考虑将数据导入数据库中，再加上持仓监控分析，就可以变成一个简单的历史回测系统了。

## 几大插件对比

看到别人[博客](http://www.gocalf.com/blog/python-read-write-excel.html "http://www.gocalf.com/blog/python-read-write-excel.html")里使用了4个python插件，在操作权限、速度等方面有所区别。

|        | XLsxWriter | xlrd&xlwt | OpenPyXL | Microsoft Excel API |
| 读     | No         | Yes       | Yes      | Yes                 |
| 写     | Yes        | Yes       | Yes      | Yes                 |
| 修改   | No         | No        | part     | Yes                 |
| .xls   | No         | Yes       | No       | Yes                 |
| .xlsx  | Yes        | part      | Yes      | Yes                 |
| 大文件 | Yes        | No        | Yes      | No                  |
| 速度   | 快         | 快        | 快       | 超慢                |
| 系统   | 无限制     | 无限制    | 无限制   | windows+excel       |

决定暂时先选择OpenPyXL来操作我的文档。

## OpenPyXL

[官方文档](https://openpyxl.readthedocs.org/en/default/ "https://openpyxl.readthedocs.org/en/default/")算是比较详细的。

### 安装

在cmd里通过pip安装，pillow包不是必须的， 如果需要用到插入图片等功能就需要安装。
 
{% highlight console %}
pip install openpyxl
pip install pillow
{% endhighlight %}

