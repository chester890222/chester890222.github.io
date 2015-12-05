---
layout: post
title: Jekyll+Github+Markdown的个人博客
author: Chen Chen
description: 利用Github Pages的免费静态网页功能，搭建个人博客，markdown编辑让你更专注于内容。
header-img: "post_img/2015/post-bg-2015.jpg"
labels: [Jekyll, Github, Markdown]
---

## 准备工作

1. Github 账号
2. Sublime Text 3 （或者其他编辑器）
3. 网站域名（不是必须）

简单说，只需要三步，就可以在 Github 搭建起一个博客：

1. 在 Github 上建一个名为 xxx.github.io 的库；
2. 把看中了的 Jekyll 模板 clone 到本地；
3. 把这个模板 push 到自己的库；

##具体步骤

### 1. 在 Github 创建名为 hyaojia.github.io 的库

首先要创建一个新的库，把它命名为 username.github.io。所以如果你的用户名是 quant222，库的名字就要 quant222.github.io。
博客搭建成功后，就可以直接通过quant222.github.io来访问你的博客了。

>其实这里的库并不一定需要用`username.github.io`来创建，也可以叫`whateveriwant`。这样的话需要在gh-pages的分支上push commits。将来访问博客则是通过`quant222.github.io/whateveriwant/`。

创建成功后，把已经创建的库 clone 到本地，或者在本地电脑保存网页的目录下用cmd输入以下命令：

{% highlight console %}
echo "# quant222.github.io" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/quant222/quant222.github.io.git
git push -u origin master
{% endhighlight %}

### 2. 选择模板

如果想从0开始学jekyll搭建网页，可以在本地创建一个空的文件夹，按照其他教程一步步创建空白博客。

更方便直观的办法就是在网上找别人已经创建完成的模版。现在 Jekyll 有很多好看的模板，可以到
[Jekyll Themes](http://jekyllthemes.org/ "http://jekyllthemes.org/")或者[知乎](http://www.zhihu.com/question/20223939 "http://www.zhihu.com/question/20223939")上找自己喜欢的风格的博客。

### 3. 把博客托管到 Github Pages

`_config.yml`里的个人信息，再把整个文件夹push到github上之前创建的库里就可以了。

模版下载非常简单可以clone或者直接下载，记得下载完把其中的.git文件给删了，是别人创建模版时历史commit等等记录，另外需要删除`_post`文档中别人的博客，再修改配置文件因模版不同而略有区别，一般需要改的有site url，github等网站账号等。

修改完成后，可以把所有文件放到之前一步在本地创建的库目录下。
之后每次更新博客，我们就需要重复 git add 和 git commit 这两步：

{% highlight console %}
$ git add . 
$ git commit -m 'modified _config.yml'
$ git push -u origin master
{% endhighlight %}

现在，进入quant222.github.com/quant222.github.io，里面应该已经有刚上传的文件了。
现在在浏览器输入网址 http://quant222.github.io，就可以看到博客的样子了（第一次上传完可能需要几分钟后才会显示）。

如果没有修改模板的需求，Jekyll+Github的个人博客就搭建完成了。

以后写文章，只需要在 _post/ 文件加中，加入带有 YAML 头信息（YAML front matter）的 markdown 文件，然后 push 到 Github，就会被自动渲染成 HTML。比如，增加一篇名为 My First Post 的博客，在本地创建一个文件名带有日期的 markdown 文件 2015-04-20-my-first-post.md（里面要写好头信息）：

{% highlight markup %}
---
layout: post
title: My First Post 
---
# 这是我的第一篇博客
{% endhighlight %}

写完文章后，按上述方法(git add commit push的顺序)推送到Github就能在网站上看到写的文章了。

