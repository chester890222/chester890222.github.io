---
layout: post
title: Jekyll+Github+Markdown的个人博客
author: Chen Chen
description: 利用Github Pages的免费静态网页功能，搭建个人博客，markdown编辑让你更专注于内容。
header-img: "post_img/2015/post-bg-2015.jpg"
tags: [Jekyll, Github, Markdown]
---

## 准备工作

1. Github 账号
2. Sublime Text 3 （或者其他编辑器）
3. 网站域名（不是必须）

简单说，只需要三步，就可以在 Github 搭建起一个博客：

1. 在 Github 上建一个名为 xxx.github.io 的库；
2. 把看中了的 Jekyll 模板 clone 到本地；
3. 把这个模板 push 到自己的库；

## 基础搭建

### 1. 在 Github 创建名为 username.github.io 的库

首先要创建一个新的库，把它命名为 username.github.io。所以如果你的用户名是 quant222（实际并不存在），库的名字就要 quant222.github.io。
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
[Jekyll Themes](http://jekyllthemes.org/ "http://jekyllthemes.org/")或者[知乎](http://www.zhihu.com/question/20223939 "http://www.zhihu.com/question/20223939")上找自己喜欢的风格的博客。我使用的是[Hux blog](http://huangxuan.me/ "http://huangxuan.me/")的模版。

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

{% highlight console %}
---
layout: post
title: My First Post 
---

# 这是我的第一篇博客

{% endhighlight %}

`需要特别注意yaml头信息里的标点，中文输入里的冒号：和双引号“”可能会使网页出错、无法正常显示等问题。`

写完文章后，按上述方法(git add commit push的顺序)推送到Github就能在网站上看到写的文章了。
至此，一个可以正常使用的个人博客搭建完成了。

## 进阶修改

由于在建这个博客前，我对javascript、css等网站设计相关语言完全不懂，只是依靠其他编程语言的一点基础，照着别人的模版稍作修改。以下的“进阶”过程只是我个人建站的记录，可能会有误导，但至少在我的网站上启动了。

### 增加个人域名

大概流程如下：

1. 注册Godaddy账户（还有其他一些域名管理的网站，如Name，万网等，可以根据自己的需要及价格选择）
2. 挑选自己心仪的域名，购买多少就看自己钱包了，Godaddy有不少promotion code，我的域名折扣之后10年60多美金。支持支付宝支付，人民币400元左右，相当于每年40几块钱，还是可以接受的。
3. 接下来就是解析DNS到自己的github page上：Godaddy自己也有DNS manager，也可以绑定到github上。但据说国内对godaddy的访问效果不好，所以通过[DNSpod](http://www.dnspod.cn "http://www.dnspod.cn")设置。
	- 首先需要在godaddy的域名设置里将nameservers修改成F1G1NS1.DNSPOD.NET和F1G1NS2.DNSPOD.NET
	- 然后进入DNSpod，添加域名记录，可参考DNSPod的[帮助文档](https://www.dnspod.cn/Support "https://www.dnspod.cn/Support")
	- 添加域名记录后，进入会有个加载配置啥的，不要保存，使用默认的两个解析就行
	- 在DNSPod自己的域名下添加一条A记录，地址就是Github Pages的服务IP地址：103.245.222.133（最好自行ping获取最新的ip,因为ip可能改变。）
	
4. 在Github项目目录下创建CNAME文件，添入刚注册的域名，push到Github上，静候DNS生效，差不多十几分钟就OK了

### pygments语法高亮

用markdown写东西的一个特色就是可以语法高亮。主要的两个插件有google code prittify和pygments。
Hux的博客里默认使用的pygments，可以在_config.yml文件里设置：`highlighter: pygments`
也可以自定义一些语法高亮的主题（Hux默认主题设置在_include文件夹的`head.html`），个人比较喜欢Monokai theme，在项目css目录下创建`monokai-syntax.css`文件，保存下面的代码

{% highlight css %}
.highlight pre { color: #fff; padding: 7px; background-color:#333}
.highlight .hll { background-color: #49483e }
.highlight .c { color: #75715e } /* Comment */
.highlight .err { color: #960050; background-color: #1e0010 } /* Error */
.highlight .k { color: #66d9ef } /* Keyword */
.highlight .l { color: #ae81ff } /* Literal */
.highlight .n { color: #f8f8f2 } /* Name */
.highlight .o { color: #f92672 } /* Operator */
.highlight .p { color: #f8f8f2 } /* Punctuation */
.highlight .cm { color: #75715e } /* Comment.Multiline */
.highlight .cp { color: #75715e } /* Comment.Preproc */
.highlight .c1 { color: #75715e } /* Comment.Single */
.highlight .cs { color: #75715e } /* Comment.Special */
.highlight .ge { font-style: italic } /* Generic.Emph */
.highlight .gs { font-weight: bold } /* Generic.Strong */
.highlight .kc { color: #66d9ef } /* Keyword.Constant */
.highlight .kd { color: #66d9ef } /* Keyword.Declaration */
.highlight .kn { color: #f92672 } /* Keyword.Namespace */
.highlight .kp { color: #66d9ef } /* Keyword.Pseudo */
.highlight .kr { color: #66d9ef } /* Keyword.Reserved */
.highlight .kt { color: #66d9ef } /* Keyword.Type */
.highlight .ld { color: #e6db74 } /* Literal.Date */
.highlight .m { color: #ae81ff } /* Literal.Number */
.highlight .s { color: #e6db74 } /* Literal.String */
.highlight .na { color: #a6e22e } /* Name.Attribute */
.highlight .nb { color: #f8f8f2 } /* Name.Builtin */
.highlight .nc { color: #a6e22e } /* Name.Class */
.highlight .no { color: #66d9ef } /* Name.Constant */
.highlight .nd { color: #a6e22e } /* Name.Decorator */
.highlight .ni { color: #f8f8f2 } /* Name.Entity */
.highlight .ne { color: #a6e22e } /* Name.Exception */
.highlight .nf { color: #a6e22e } /* Name.Function */
.highlight .nl { color: #f8f8f2 } /* Name.Label */
.highlight .nn { color: #f8f8f2 } /* Name.Namespace */
.highlight .nx { color: #a6e22e } /* Name.Other */
.highlight .py { color: #f8f8f2 } /* Name.Property */
.highlight .nt { color: #f92672 } /* Name.Tag */
.highlight .nv { color: #f8f8f2 } /* Name.Variable */
.highlight .ow { color: #f92672 } /* Operator.Word */
.highlight .w { color: #f8f8f2 } /* Text.Whitespace */
.highlight .mf { color: #ae81ff } /* Literal.Number.Float */
.highlight .mh { color: #ae81ff } /* Literal.Number.Hex */
.highlight .mi { color: #ae81ff } /* Literal.Number.Integer */
.highlight .mo { color: #ae81ff } /* Literal.Number.Oct */
.highlight .sb { color: #e6db74 } /* Literal.String.Backtick */
.highlight .sc { color: #e6db74 } /* Literal.String.Char */
.highlight .sd { color: #e6db74 } /* Literal.String.Doc */
.highlight .s2 { color: #e6db74 } /* Literal.String.Double */
.highlight .se { color: #ae81ff } /* Literal.String.Escape */
.highlight .sh { color: #e6db74 } /* Literal.String.Heredoc */
.highlight .si { color: #e6db74 } /* Literal.String.Interpol */
.highlight .sx { color: #e6db74 } /* Literal.String.Other */
.highlight .sr { color: #e6db74 } /* Literal.String.Regex */
.highlight .s1 { color: #e6db74 } /* Literal.String.Single */
.highlight .ss { color: #e6db74 } /* Literal.String.Symbol */
.highlight .bp { color: #f8f8f2 } /* Name.Builtin.Pseudo */
.highlight .vc { color: #f8f8f2 } /* Name.Variable.Class */
.highlight .vg { color: #f8f8f2 } /* Name.Variable.Global */
.highlight .vi { color: #f8f8f2 } /* Name.Variable.Instance */
.highlight .il { color: #ae81ff } /* Literal.Number.Integer.Long */

.highlight .gh { } /* Generic Heading & Diff Header */
.highlight .gu { color: #75715e; } /* Generic.Subheading & Diff Unified/Comment? */
.highlight .gd { color: #f92672; } /* Generic.Deleted & Diff Deleted */
.highlight .gi { color: #a6e22e; } /* Generic.Inserted & Diff Inserted */
{% endhighlight %}

之后在_include文件夹下的`footer.html`里添加

{% highlight html %}
    <link rel="stylesheet" href="{{ "/css/monokai-syntax.css" | prepend: site.baseurl }}">
    <script type="text/javascript" src="http://cdn.bootcss.com/highlight.js/8.9.1/highlight.min.js"></script>
    <script>hljs.initHighlightingOnLoad();</script>
{% endhighlight %}


### 添加back-to-top按钮

在页面右下角添加一个返回页首的按钮，看上去挺实用的，所以就模仿了一个。

首先,网上找一张back-to-top的图片放在img文件夹下。
在css文件夹下创建`backtop.css`（也可以放在其他的css文件里）

{% highlight css %}

#back-top {
  position: fixed;
  bottom: 30px;
  margin-left: 1040px;
}
#back-top a {
  width: 54px;
  height: 54px;
  display: block;
  background: #ddd url(../img/back-to-top.png) no-repeat center center;
  background-color: #ddd;
  -webkit-border-radius: 7px;
  -moz-border-radius: 7px;
  border-radius: 7px;
  -webkit-transition: 1s;
  -moz-transition: 1s;
  transition: 1s;
}
#back-top a:hover {
  background-color: #777;
}

@media (min-width: 768px) {
#back-top {
  display: block;
}
}
{% endhighlight %}

在js文件夹下创建`backtop.js`

{% highlight js %}
$("#back-top").hide();
$(document).ready(function () {
  $(window).scroll(function () {
    if ($(this).scrollTop() > 100) {
      $('#back-top').fadeIn();
    } else {
      $('#back-top').fadeOut();
    }
  });
  $('#back-top a').click(function () {
    $('body,html').animate({
      scrollTop: 0
    }, 800);
    return false;
  });
});
{% endhighlight %}

然后，在`footer.html`里加上

{% highlight html %}
<script src="{{ "/js/backtop.js " | prepend: site.baseurl }}"></script>
{% endhighlight %}

最后，在想让它出现的位置，比如`post.html`里加入

{% highlight html %}
  <div id="back-top" class="hidden-print hidden-sm hidden-xs">
       <a href="#top" title="Back to top"></a>
  </div>
{% endhighlight %}

### 添加table of content文章目录的sidebar

目标是在页面足够的情况下，文章的右侧出现该文章的目录，并悬浮窗口。
参考自 [Thomas Zhao Studio](http://www.thomaszhao.cn/2015/01/08/how-do-i-build-this-jekyll-blog/#section-4 "http://www.thomaszhao.cn/2015/01/08/how-do-i-build-this-jekyll-blog/#section-4")

在css文件夹下创建`toc.css`（也可以放在其他的css文件里）

{% highlight css %}
/* By default it's not affixed in mobile views, so undo that */
.bs-docs-sidebar.affix {
  position: static;
}
@media (min-width: 768px) {
  .bs-docs-sidebar {
    padding-left: 10px;
  }
}

/* First level of nav */
.bs-docs-sidenav {
  margin-top: 40px;
  margin-bottom: 20px;
}

/* All levels of nav */
.bs-docs-sidebar .nav > li > a {
  display: block;
  padding: 4px 10px;
  font-size: 20px;
  font-weight: 300;
  color: #696969;
  /*no wrap*/
  overflow:hidden;
  white-space:nowrap;
  text-overflow:ellipsis
}
.bs-docs-sidebar .nav > li > a:hover,
.bs-docs-sidebar .nav > li > a:focus {
  padding-left: 10px;
  color: #563d7c;
  text-decoration: none;
  background-color: transparent;
  border-left: 1px solid #563d7c;
}
.bs-docs-sidebar .nav > .active > a,
.bs-docs-sidebar .nav > .active:hover > a,
.bs-docs-sidebar .nav > .active:focus > a {
  padding-left: 9px;
  font-weight: 300;
  color: red;
  background-color: transparent;
  border-left: 2px solid #563d7c;
}

/* Nav: second level (shown on .active) */
.bs-docs-sidebar .nav .nav {
  /* display: none; Hide by default, but at >768px, show it */
  /* padding-bottom: 10px; */
}
.bs-docs-sidebar .nav .nav > li > a {
  padding-top: 1px;
  padding-bottom: 1px;
  padding-left: 20px;
  font-size: 18px;
  font-weight: bold;
}
.bs-docs-sidebar .nav .nav > li > a:hover,
.bs-docs-sidebar .nav .nav > li > a:focus {
  padding-left: 19px;
}
.bs-docs-sidebar .nav .nav > .active > a,
.bs-docs-sidebar .nav .nav > .active:hover > a,
.bs-docs-sidebar .nav .nav > .active:focus > a {
  padding-left: 18px;
  font-weight: bold;
  color: red;
}

/* Nav: 3rd level (shown on .active) */
.bs-docs-sidebar .nav .nav .nav {
  /*display: none;  Hide by default, but at >768px, show it */
  /* padding-bottom: 10px;*/
}
.bs-docs-sidebar .nav .nav .nav > li > a {
  padding-top: 1px;
  padding-bottom: 1px;
  padding-left: 30px;
  font-size: 17px;
  font-weight: normal;
}
.bs-docs-sidebar .nav .nav .nav > li > a:hover,
.bs-docs-sidebar .nav .nav .nav > li > a:focus {
  padding-left: 29px;
}
.bs-docs-sidebar .nav .nav .nav > .active > a,
.bs-docs-sidebar .nav .nav .nav > .active:hover > a,
.bs-docs-sidebar .nav .nav .nav > .active:focus > a {
  padding-left: 28px;
  font-weight: bold;
  color: red;
}
{% endhighlight %}

在js文件夹下创建`toc.js`

{% highlight js %}

var BlogDirectory = {

    /*
     * Side Nav Affixing
     *
     * reference to:
     * https://github.com/twbs/bootstrap/blob/master/docs/assets/js/src/application.js#L35
     */
    setSideNavAffixing:function() {

        // Scrollspy
        var $window = $(window)
        var $body   = $(document.body)

        $body.scrollspy({
            target: '.bs-docs-sidebar'
        })
        $window.on('load', function () {
            $body.scrollspy('refresh')
        })

        // Kill links
        $('.bs-docs-container [href=#]').click(function (e) {
            e.preventDefault()
        })

        // Sidenav affixing
        setTimeout(function () {
            var $sideBar = $('.bs-docs-sidebar')

            $sideBar.affix({
                offset: {
                    top: function () {
                        var offsetTop      = $sideBar.offset().top
                        var sideBarMargin  = parseInt($sideBar.children(0).css('margin-top'), 10)
                        var navOuterHeight = $('.bs-docs-bar-top').height()

                        return (this.top = offsetTop - navOuterHeight - sideBarMargin)
                    },
                    bottom: function () {
                        return (this.bottom = 0 - $('.bs-docs-container').outerHeight(true))
                    }
                }
            })
        }, 100)

        setTimeout(function () {
            $('.bs-top').affix()
        }, 100)
    }, // end of setSideNavAffixing:function()


    /* Generate directory tree
     *
     * side_nav_element: side navigation element
     * article_body_element:  article body container.
     *
     * processing: search header elements(h1,h2,h3) in `article_body_element`,
     * generate directory tree list, and put them into side_nav_element.
     */
    createBlogDirectory:function (side_nav_element, article_body_element){
        if(!article_body_element || article_body_element.length < 1 ||
                !side_nav_element) {
            return false;
        }

        var nodes = article_body_element.find("h1,h2,h3");
        var ulSideNav = side_nav_element;

        $.each(nodes,function(){
            var $this = $(this);

            var nodetext = $this.text();
            // There maybe HTML tags in header inner text, use regex to erase them
            nodetext = nodetext.replace(/<\/?[^>]+>/g,"");
            nodetext = nodetext.replace(/&nbsp;/ig, "");

            // btw: Jekyll generates id for each header.
            var nodeid = $this.attr("id");
            if(!nodeid) {
                nodeid = "top";
            }

            var item_a = $("<a></a>");
            item_a.attr("href", "#" + nodeid);
            item_a.text(nodetext);

            var ret_li;
            // wrapper: ul ( in the template, outside this code )
            // h1: layer 1: li - a
            // h2: layer 2: ul - li - a
            // h3: layer 3: ul - ul - li - a
            switch($this.get(0).tagName) {
            case "H1":
                var li_a = $("<li></li>").append(item_a);
                ret_li = li_a;
                break;
            case "H2":
                var li_a = $("<li></li>").append(item_a);
                var nav_li_a = $("<ul class=\"nav\"></ul>").append(li_a);
                ret_li = nav_li_a;
                break;
            case "H3":
                var li_a = $("<li></li>").append(item_a);
                var nav_li_a = $("<ul class=\"nav\"></ul>").append(li_a);
                var nav_nav_li_a = $("<ul class=\"nav\"></ul>").append(nav_li_a);
                ret_li = nav_nav_li_a;
                break;
            }

            if(!ret_li) {
                // do nothing
            } else {
                ulSideNav.append(ret_li);
            }
        })  // end of each

    } //end of createBlogDirectory:function()

};


jQuery(function($) {
    $(document).ready( function() {
        /* qrcode
        var opt = { text : window.location.href, width:150, height:150 };
        try {
            document.createElement("canvas").getContext("2d");
        } catch (e) {
            $.extend(opt,{ render : "table" });
        }
        $('.qrcode').html('').qrcode(opt);
    */

        // Generate the side navigation `ul` elements
//        BlogDirectory.createBlogDirectory($("#sideNav"), $(".bs-docs-container"));
        BlogDirectory.createBlogDirectory($("#sideNav"), $(".post-container"));

        // caculate affixing
        BlogDirectory.setSideNavAffixing();
    });
});

{% endhighlight %}

然后，在`footer.html`里加上

{% highlight html %}
<!-- TOC sidebar-->
<script src="{{ "/js/toc.js " | prepend: site.baseurl }}"></script>
{% endhighlight %}

最后，在`post.html`里加入

{% highlight html %}
{% unless page.fullwidth == true %}
<nav class="bs-docs-sidebar ">
<ul id="sideNav" class="nav bs-docs-sidenav">

<!-- code will be generated in application.js -->

</ul>
</nav>
{% endunless %}
{% endhighlight %}

（可能需要对原来的post位置做一些调整）
