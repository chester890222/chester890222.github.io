---
layout: post
title: pip用法记录
author: Chen Chen
description: "pip的常见用法"
header-img: "img/python-bg.jpg"
tags: [Python]
---


pip升级自己： 
 
{% highlight console %}
pip install --upgrade pip
{% endhighlight %}

查找与安装：
使用search、install这两个参数。

查看某个库的信息： 
 
{% highlight console %}
pip show Jinja2
---
Name: Jinja2
Version: 2.7.3
Location: /path/to/virtualenv/lib/python2.7/site-packages
Requires: markupsafe
{% endhighlight %}

查看已经安装的库： 

{% highlight console %}
pip list
{% endhighlight %}

获取过期的库：
 
{% highlight console %}
pip list --outdated
pip list --outdated | grep Jinja2
{% endhighlight %}


