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

网上看到别人提到的4个python插件，在操作权限、速度等方面有所区别

### XlsxWriter

`XlsxWriter只能用来写文件。`其官方文档中宣称它支持：
- 100% compatible Excel XLSX files.
- Full formatting.
- Merged cells.
- Defined names.
- Charts.
- Autofilters.
- Data validation and drop down lists.
- Conditional formatting.
- Worksheet PNG/JPEG images.
- Rich multi-format strings.
- Cell comments.
- Memory optimisation mode for writing large files.

#### 优点

1. 功能比较强
相对而言，这是除Excel自身之外功能最强的工具了。比如我就用到了它提供的：字体设置、前景色背景色、border设置、视图缩放（zoom）、单元格合并、autofilter、freeze panes、公式、data validation、单元格注释、行高和列宽设置等等。
用它生成的带有单元格注释的Excel文件，不论是Excel 2007还是Excel 2013都可正常打开（下面会提到，这个任务用Excel自身都无法完成）。

2. 支持大文件写入
如果数据量非常大，可以启用constant memory模式，这是一种顺序写入模式，得到一行数据就立刻写入一行，而不会把所有的数据都保持在内存中。

#### 缺点

1. 不支持读取和修改
作者并没有打算做一个XlsxReader来提供读取操作。不能读取，也就无从修改了。它只能用来创建新的文件。可以利用xlrd把需要的信息读入后，用XlsxWriter创建全新的文件。
另外，即使是创建到一半Excel文件，也是无法读取已经创建出来的内容的（信息应该在，但是并没有相应的接口）。因为它的主要方法是write而不是set。当你在某个单元格写入数据后，除非你自己保存了相关的内容，否则还是没有办法读出已经写入的信息。从这个角度看，你无法做到读出->修改->写回，只能是写入->写入->写入。

2. 不支持XLS文件

3. 暂时不支持透视表（Pivot Table）


### xlrd&xlwt

xlrd&xlwt主要是针对Office 2013或更早版本的XLS文件格式。

#### 优点

1. 支持XLS格式
XlsxWriter和OpenPyXL都不支持XLS格式，从这个角度看，xlrd&xlwt仍然有一定的不可替代性。

#### 缺点

1. 对XLSX支持比较差
目前xlrd已经可以读取XLSX文件了，有限地支持。

2. 对修改的支持比较差
xlrd和xlwt是两个相对独立的模块，虽然xlutils提供方法帮助你把xlrd.Book对象复制到xlwt.Workbook对象，但跟XlsxWriter类似，后者只是提供write方法，使得你无法很容易地获取当前已经写入的数据并进行有针对性的修改。如果非要这样做，你要不断地保存，然后再用新的xlrd.Book对象读取你要的信息，还是比较麻烦的。

3. 功能很弱
除了最基本的写入数据和公式，xlwt所提供的功能非常少（Excel 2013本身支持的功能也就很少）。对于读取也是一样的，很多信息在读入时就丢失掉了。

### OpenPyXL

OpenPyXL是比较综合的一个工具，能读能写能修改，功能还算可以但也有很大的缺陷。

#### 优点

1. 能读能写能修改
OpenPyXL的工作模式跟XlsxWriter和xlwt有很大的区别，它用的是getter/setter模式。你可以随时读取某个单元格的内容，并根据其内容进行相应的修改，OpenPyXL会帮你记住每个单元格的状态。

2. 功能还算可以
整体来讲，它所支持的功能介于XlsxWriter和xlwt之间。

>特别需要注意的一点：虽然它支持修改已有文件，但由于其所支持的功能有限，读入文件时会忽略掉它所不支持的内容，再写入时，这些内容就丢失了。因此使用时一定要慎重。比如下面的缺点中提到它无法读入公式，那`如果你修改一个带有公式的文件，保存之后，所有的公式就都没有了。`

#### 缺点

1. 不支持XLS

2. 不支持读取公式
Excel的单元格如果是一个公式，它内部会同时保存公式本身和运算结果的缓存。用OpenPyXL读取单元格内容，它不会告诉你这个单元格的公式是什么，甚至不会告诉你这个单元格存的是公式，它只会拿到这个缓存的运算结果。我本来想利用它判别单元格是不是用了公式，然后做出不同的处理。结果遇到了这个问题，最后只好采取了其他变通的方式去做。

### Microsoft Excel API
大部分Windows环境的开发人员都会选择Microsoft Excel API。实际上不仅仅是Python，几乎各种语言都有相应的方法使用它，因为核心的逻辑完全是由Microsft Excel自身提供的。语言相关的部分只是负责跟Windows的COM组件进行通信。

在Python中首先需要安装Python for Windows extensions（pywin32），具体的文档可以查阅Win32 Modules和Python COM。

当然你还必须要安装某一个版本的Microsoft Office Excel，它内部的DLL负责实际的操作。

#### 优点

1. 最大的优点：强大无极限
因为直接与Excel进程通信，你可以做任何在Excel里可以做的事情。

2. 文档丰富
MSDN上的文档绝对是世界上最优秀的文档。没有之一。

3. 调试方便
你完全可以直接在Excel里面用宏先调试你想要的效果。甚至如果你不清楚怎么用程序实现某个操作，你可以通过宏录制的方法得到该操作的处理代码。

#### 缺点

1. 致命的缺点：慢
因为需要与Excel进程通信，其效率是非常低的。

2. 平台限制
目前还没有发现可以在非Windows系统使用它的方法。
另外，基于它的程序能做什么事情，很大程度上依赖于当前系统所安装的Excel版本。不同的版本在功能上有很大的差异，API也会有差异。用起来会比较麻烦。

如果让Excel窗口可见，随着程序的运行，你可以看到每一句程序所带来的变化，单元格的内容一个一个地改变。如果要写入的数据很多，那速度是无法忍受的。
