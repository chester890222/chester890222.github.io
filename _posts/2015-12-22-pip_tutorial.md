---
layout: post
title: pip用法记录
author: Chen Chen
description: "pip的常见用法"
header-img: "img/python-bg.jpg"
tags: [Python]
---

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

获取并更新过期的库：
 
{% highlight console %}
pip list --outdated
pip install --upgrade package_name
{% endhighlight %}


pip升级自己： 
 
{% highlight console %}
python -m pip install --upgrade pip
{% endhighlight %}


pip的参数解释：
 
{% highlight console %}
pip --help
 
Usage:   
  pip <command> [options]
 
Commands:
  install                     安装包.
  uninstall                   卸载包.
  freeze                      按着一定格式输出已安装包列表
  list                        列出已安装包.
  show                        显示包详细信息.
  search                      搜索包，类似yum里的search.
  wheel                       Build wheels from your requirements.
  zip                         不推荐. Zip individual packages.
  unzip                       不推荐. Unzip individual packages.
  bundle                      不推荐. Create pybundles.
  help                        当前帮助.
 
General Options:
  -h, --help                  显示帮助.
  -v, --verbose               更多的输出，最多可以使用3次
  -V, --version               现实版本信息然后退出.
  -q, --quiet                 最少的输出.
  --log-file <path>           覆盖的方式记录verbose错误日志，默认文件：/root/.pip/pip.log
  --log <path>                不覆盖记录verbose输出的日志.
  --proxy <proxy>             Specify a proxy in the form [user:passwd@]proxy.server:port.
  --timeout <sec>             连接超时时间 (默认15秒).
  --exists-action <action>    Default action when a path already exists: (s)witch, (i)gnore, (w)ipe, (b)ackup.
  --cert <path>               证书.
{% endhighlight %}
