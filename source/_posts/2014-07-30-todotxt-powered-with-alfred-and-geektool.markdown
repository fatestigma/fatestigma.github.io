---
layout: post
title: "todo.txt powered with Alfred&GeekTool"
date: 2014-07-30 21:51:16 +0800
comments: true
categories: 
tags: 
---
在经过上一篇博客[使用GeekTool来美化Mac的桌面]之后，发现[GeekTool]越发是一个方便的好东西。
在最近经过各种思想斗争后决定放弃OmniFocus之后，也在一直寻找一个更适合我的GTD软件。
偶然间就遇到了[Todo.txt]这个非常简便的文本GTD应用。
研究在三后，突发奇想觉得可以用GeekTool把任务都放在桌面上，再加上之前一直对Alfred的热爱，
由此开始制作了这个。

其实内容很简单，就是把一个Shell Geeklets作为终端显示器，显示`todo.sh`的结果。
但是shell Geeklets的代码是一成不变的，所以就新建`/tmp/display.txt`这个文件用来做内容中转站。
这样Geeklets的代码就很简单了。
{% codeblock lang:bash %}
cat /tmp/display.txt
{% endcodeblock %}
就ok了。

然后就是开始通过Alfred来定义各种功能的Keyword。下面就是主要功能介绍了。

>| todo {task}		| 添加一个任务 							|
| done {n#}			| 将标号n#的任务标记为完成					|
| todel {n#}		| 删除标号n#的任务						|
| tols [key]		| 显示所有任务，或与key相关的任务			|
| tolsc [key]		| 显示上下文(Context)，用key来筛选上下文	|
| tolsprj [key]	　　| 显示项目(Project)，用key来筛选项目		|
| topri [key]		| 升高指定任务的优先级						|
| todpri [key]		| 降低指定任务的优先级						|
| toedit			| 直接编辑`todo.txt`文件					|

<!-- more -->

Todo.txt的使用介绍：

	# 添加一个任务, +project, @context
	a "THING I NEED TO DO [+project] [@context]"
	# 添加一个任务, 优先级设置为最高
	a (A) THING I NEED TO DO
	# 添加多条任务
    addm "FIRST THING I NEED TO DO +project1 @context
    SECOND THING I NEED TO DO +project2 @context"
	
	# 显示所有任务或者显示有关TERM的任务，用-TERM来屏蔽有关TERM的任务
	ls [TERM...]
	# 显示包括已完成任务的所有任务
	lsa [TERM...]
	# 显示所有上下文
	lsc [TERM...]
	# 显示所有项目
	lsprj [TERM...]
	# 显示所有优先任务，用PRIORITIES来显示某一优先级任务，用TERM筛选
	lsp [PRIORITIES] [TERM...]
	
	# 标记任务已完成
	do ITEM#[, ITEM#, ITEM#, ...]
	# 删除一个任务，如果有TERM则仅删除任务中的指定字段
	del|rm ITEM# [TERM]
	# 去除重复条目
	deduplicate
	
	# 更改一个任务
	replace ITEM# "UPDATED TODO"
	# 为指定任务定义优先级
	p ITEM# PRIORITY
	# 去除指定任务的优先级
	dp ITEM#[, ITEM#, ITEM#, ...]
	# 在指定任务尾部添加TEXT
	app ITEM# "TEXT TO APPEND"
	# 在指定任务首部添加TEXT
	prep ITEM# "TEXT TO PREPEND"
	
当然，以上都是针对单一文件的操作。对于多个todo文件的就比较麻烦了。。

Todo.txt作为一个开源的应用所以也有着很多add-on，[Todo.sh Add on Directory]。



[使用GeekTool来美化Mac的桌面]:{{ root_url }}/blog/2014/07/28/geektool-beautify-mac-desktop/
[Todo.txt]:https://github.com/ginatrapani/todo.txt-cli
[GeekTool]:http://projects.tynsoe.org/en/geektool/
[Todo.sh Add on Directory]:https://github.com/ginatrapani/todo.txt-cli/wiki/Todo.sh-Add-on-Directory
