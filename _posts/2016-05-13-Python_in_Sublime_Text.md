---
layout: post
title: Sublime Text 3 Python
author: Chen Chen
description: "用Sublime Text来编写Python时用到的一些插件和用法。"
header-img: "img/python-bg.jpg"
tags: ["Python", "Sublime Text"]
---

### SublimeREPL

通过这个插件就可以在Sublime中直接运行python代码了。要运行的时候选Tools->SublimeREPL->Python就可以看到了。
每次都要点不方便，可以在Sublime中通过Preferences->Key Bindings - User设置快捷键

{% highlight json %}
[
    //SublimeREPL
    {"keys":["f5"],  
    "caption": "SublimeREPL: Python - RUN current file",  
    "command": "run_existing_window_command",  
    "args":  
    {  
        "id": "repl_python_run",  
        "file": "config/Python/Main.sublime-menu"  
    }},  
    {"keys":["f4"],  
     "caption": "SublimeREPL: Python - IPython",  
     "command": "run_existing_window_command", "args":  
     {  
         "id": "repl_python_ipython",  
         "file": "config/Python/Main.sublime-menu"  
     }}  ,
]
{% endhighlight %}

我喜欢用Ipython，可以选择运行单行或者一段代码，在开发的时候会方便很多。快捷键是ctrl加逗号，再按s、l、f、b。

- ctrl + , , s 运行选中部分
- ctrl + , , l 运行单行
- ctrl + , , f 运行文件
- ctrl + , , b 运行block


### SublimeCodeIntel 插件

智能提示插件，据说这个插件的智能提示功能非常强大，可以自定义提示的内容库。

{% highlight console %}
"Python": {
        "python":"D:/Python34python.exe",
        "pythonExtraPaths":
            [
                "D:/Python34",
                 "D:/Python34/DLLs",
                 "D:/Python34/Lib",
                 "D:/Python34/Lib/lib-tk",
                 "D:/Python34/Lib/site-packages"
            ]
        }
{% endhighlight %}

可能需要另外设置才能流畅使用，我暂时没有使用。


### Python PEP8 Autoformat 插件

这是用来按PEP8自动格式化代码的。快捷键 CTRL+SHIFT+R 自动格式化python代码
 
{% highlight css %}
 {
     "auto_complete": false,
     "caret_style": "solid",
     "ensure_newline_at_eof_on_save": true,
     "find_selected_text": true,
     "font_size": 11.0,
     "highlight_modified_tabs": true,
     "line_padding_bottom": 0,
     "line_padding_top": 0,
     "scroll_past_end": false,
     "show_minimap": false,
     "tab_size": 4,
     "translate_tabs_to_spaces": true,
     "trim_trailing_white_space_on_save": true,
     "wide_caret": true,
     "word_wrap": true,
 }
{% endhighlight %}

之前一直使用这个插件，Anaconda中也带了类似的功能。


### Anaconda

这个Anaconda不是那个python的集成科学计算库，而是sublime的一个插件。是一款非常强大的python自动补全和语法提示插件，实现了不少IDE的功能。很多功能都可以直接通过右键的Anaconda选项下打开。

#### 让Anaconda以dot为命令补全触发器

需要在用户自定义设置中添加一句话：

 
{% highlight json %}
{
    "auto_complete_triggers": [{"selector": "source.python - string - comment
    - constant.numeric", "characters": "."}]
}
{% endhighlight %}

这个用户自定义设置有两个选择，你可以直接在Preferences - Settings - User里面添加这么一句，也可以在Preferences - Browse Packages...打开的文件夹里面新建一个叫做Python.sublime-settings的文件，并把上面的语句添加在里面。

#### 让回车不再输错代码

sublime在代码补全的时候默认会用tab和enter作为确认的按键，所以经常误输入一些东西（特别是添加了以dot作为命令补全的触发器之后），那么就得让enter不再作为代码补全的确认按键。

方法也很简单，首先在用户自定义设置（Preferences - Settings-User）里面添加一个配置：
 
{% highlight json %}
"auto_complete_commit_on_tab": true,
{% endhighlight %}

然后在Preferences - KeyBindings-User中添加如下配置：

 
{% highlight json %}
{ "keys": ["enter"], "context":
    [
        { "key": "setting.auto_complete_commit_on_tab", "operand": true }
    ]
},
{% endhighlight %}


其实大家可以看到Preferences - KeyBindings-Default里面有这么一段，但是这个Default文件是无法更改的，所以就要放到User里面去。
 
{% highlight json %}
{ "keys": ["enter"], "command": "commit_completion", "context":
        [
            { "key": "auto_complete_visible" },
            { "key": "setting.auto_complete_commit_on_tab", "operand": false }
        ]
},
{% endhighlight %}

#### PEP8自动格式化宏

打开录制宏（Tools - Record Macro）之后，做两部操作，第一步全选（快捷键Ctrl + A），第二步PEP8格式化（默认快捷键Ctrl + Alt + R），然后结束录制宏（Tools - Stop Record Macro），并且把宏存储为一个文件PEE8Format.sublime-macro，然后绑定一个快捷键到这个宏文件上。在KeyBindings-User里面添加：
 
{% highlight json %}
{
    "keys": ["f1"],
    "command": "run_macro_file", 
    "args":{"file": "Packages/User/PEE8Format.sublime-macro"}
}
{% endhighlight %}


另外，由于PEP8规定缩进是以4个空格为单位的，虽然tab运行起来没错，但是Anaconda会提醒你不符合PEP8标准，所以还是把tab直接替换为4个空格吧。我们需要在Setting-User里面添加两句话，含义显而易见的两句话。
 
{% highlight json %}
"tab_size": 4,
"translate_tabs_to_spaces": true
{% endhighlight %}

