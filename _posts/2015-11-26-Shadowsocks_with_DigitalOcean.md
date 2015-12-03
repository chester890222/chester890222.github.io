---
layout: post
title: 使用DigitalOcean和shadowsocks来科学上网
author: Chen Chen
description: 通过海外vps构建科学上网，大大提升工作学习生活质量。
header-img: "post_img/2015/post-bg-2015.jpg"
labels: [shadowsocks, "DigitalOcean", VPS]
---

没有google的日子实在是太痛苦了。只能用百度搜索的结果就是工作学习效率大打折扣。陆陆续续用过不少科学上网的手段，但都维持不了太长时间。于是出于爱折腾的精神，决定自己用vps架代理。

最初买完VPS想着搭建VPN，但由于自己对服务器系统毫无基础，在尝试了多个教程后决定暂时放弃。感谢 [Jerry的教程](http://jerryzou.com/posts/shadowsocks-with-digitalocean/ "http://jerryzou.com/posts/shadowsocks-with-digitalocean/")，不到10分钟就能连上google了。以下内容基于他的教程的修改。


## DigitalOcean

首先注册账号（通过[邀请链接](www.digitalocean.com/?refcode=2771cda1acf0 "www.digitalocean.com/?refcode=2771cda1acf0")可以收到\\$10的奖励)，激活邮箱自不必说。激活后就会收到奖励，但是仅仅这样还是无法在Digital Ocean上创建虚拟主机。你需要绑定信用卡或者直接向你的账户里冲入\\$5才能正式开始使用。我是在paypal账号上直接用信用卡支付的，支持人民币。

然后就可以创建你的云主机了，选择\\$5/mon的那一档来支撑shadowsocks的服务足矣。经过测试San Francisco的机房延迟最低，平均在230ms左右。而Singapore的机房延迟在280ms，还有5%左右的丢包率。所以经过几次创建后又销毁重新创建地倒腾，我最后还是选择了使用San Francisco的节点。另外操作系统的话，我选择的是`ubuntu 14.04 x64`的。

### 创建SSH key

接下来可以添加SSH key，这一步不是必须的，但是我觉得使用SSH key比使用Digital Ocean为你创建的随机密码好一点。如果不想做这一步，或者你之前已经创建过SSH key的话，可以跳过这一部分。

#### 什么是SSH key

SSH key是一个简单而又安全地连接到你的远端设备的方式，通过它你不需要在网络上传输你的密码。SSH key有public和private两部分，其中private部分存储在你的设备本地，而public部分则需要上传到远程设备上。当你通过ssh连接到远程设备上时，只有私钥和公钥匹配上才能登陆。

#### 如何创建SSH key

第一步，首先查看你本地设备上是否有SSH keys。你可以运行以下指令：

{% highlight console %}
cd ~/.ssh
ls *.pub
{% endhighlight %}

如果没有任何输出，说明你需要创建一个新的SSH key：

{% highlight console %}
ssh-keygen -t rsa -C "email@example.com"
{% endhighlight %}

后面的email请替换成你自己的email。接着你就会看到类似下面的信息：

{% highlight console %}
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/you/.ssh/id_rsa): [Press enter]
Enter passphrase (empty for no passphrase): [Type a passphrase]
Enter same passphrase again: [Type passphrase again]
Now your SSH key will be generated.
Your identification has been saved in /Users/your_username/.ssh/id_rsa.
Your public key has been saved in /Users/your_username/.ssh/id_rsa.pub.
The key fingerprint is:
01:0f:f4:3b:ca:85:d6:17:a1:7d:f0:68:9d:f0:a2:db email@example.com
{% endhighlight %}

比如我的公钥就生成于：/Users/Jerry/.ssh/id_rsa.pub，接下来就可以把公钥内容传到Digital Ocean上了。

## Shadowsocks

其实在shadowsocks的[github主页](https://github.com/shadowsocks/shadowsocks)上有很详细地说明，但是为了本文的完整性，还是将这些操作放在本文中以供参考。

### 在服务器端安装shadowsocks

首先需要远程登录到Digital Ocean的主机上，因为之前已经建好了SSH Key，所以直接用root用户登录即可：

{% highlight console %}
ssh root@VPS IP address
{% endhighlight %}

在 Debian / Ubuntu 下 安装shadowsocks

{% highlight console %}
apt-get install python-pip
pip install shadowsocks
{% endhighlight %}

我在实际安装下发现很多依赖缺失，所以需要先执行一下：apt-get update。另外也有一些同学会选择CentOS的服务器，附上在CentOS下安装shadowsocks的方法：

{% highlight console %}
yum install python-setuptools && easy_install pip
pip install shadowsocks
{% endhighlight %}

### 启动shadowsocks服务

安装好shadowsocks以后，启动shadowsocks服务可以通过以下指令：

{% highlight console %}
ssserver -p 8836 -k `password` -m rc4-md5

#或者可以通过以下指令在后台启动shadowsocks的服务：
ssserver -p 8836 -k `password` -m rc4-md5 -d start
ssserver -p 8836 -k `password` -m rc4-md5 -d stop
{% endhighlight %}

但上面的方法很不方便，我还是推荐使用配置文件的方法。首先创建一个文件：/etc/shadowsocks.json，示例如下：

{% highlight json %}
{
    "server":"你的服务器ip地址",
    "server_port":8388,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"你设置的密码",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
}
{% endhighlight %}

接下来你就可以使用下面这个指令启动服务

{% highlight console %}
ssserver -c /etc/shadowsocks.json

#或者在后台运行
ssserver -c /etc/shadowsocks.json -d start
ssserver -c /etc/shadowsocks.json -d stop
{% endhighlight %}

### 使用shadowsocks客户端

shadowsocks的客户端支持各大主流平台，而且客户端的配置一般都很简单，只需要配置一下服务器的ip地址和之前设置好的连接密码即可。