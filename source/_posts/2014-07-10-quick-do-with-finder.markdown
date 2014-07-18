---
layout: post
title: "Alfred Workflows: Quick Do"
date: 2014-07-10 03:20:19 +0800
comments: true
categories: Workflows
tags: Alfred Python AppleScript
---

这个Workflow是对上一篇博客[在终端中快速进入当前目录]的一个扩展，

**使用方法:**

>* `⌘R` 快速在iTerm中到达当前路径，如果有源文件被选中可直接编译运行  
* `⌘E` 在指定的编辑器中编辑当前源文件，与原生的`⌘O`并不相同  
* `⌥⌘E` 在iTerm中使用Vim编辑当前源文件  
* `⌥N` 在当前路径下新建一个命名文档，并在指定的编辑器中打开
* `new [name]` 与`⌥N`功能相同

<!--more-->

`⌘R`对于不同文件的编译方法需要自己编辑，比如选中`makefile`则直接执行`make`；  
选中`somescript.py`，则执行`python somescript.py`；  
选中`file.tex`，我选择的是`xelatex`，则执行`xelatex -shell-escape -8bit file.tex`。

当然功能上也不仅限于此，对于不同类型的文件可以预先写入不同的声明，
比如在`py`后缀的新文件中预先写入
{% codeblock lang:py %}
#!/usr/bin/python
# -*- coding: uft-8 -*-
{% endcodeblock %}
这个我之前在我的`.vimrc`中也添加了，非常方便。

其中`⌥N`和`new`指令的参数缺省功能相同，会有Dialog要求输入文件名;
`new`指令如果已在参数中输入文件名则不会再次询问；
如果文件已存在则会直接打开文件。

**[点此下载]**  
**[GitHub]**


[在终端中快速进入当前目录]:{{ root_url }}/blog/2014/07/07/quick-access-to-current-folder-in-shell/
[点此下载]:https://raw.githubusercontent.com/fatestigma/AlfredWorkflows/master/Downloads/Quick%20Do%20v0.1%20dev.alfredworkflow
[GitHub]:https://github.com/fatestigma/AlfredWorkflows

