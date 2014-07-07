---
layout: post
title: "Xcode5搭建Python"
date: 2014-05-01 18:51:45 +0800
comments: true
categories: Python
tags: [Python, Xcode]
---

其实也是一时兴起想要看看能不能在Xcode上运行一下其他语言的代码。
结果还真叫我找到了方法，参考与Stack Overflow上的一个问题[Python in Xcode4 or Xode5][1]

###步骤：<!--more-->
1. 首先在电脑上找到python的安装路径，在Terminal或[iTerm][2]里输入 `$ which python`
2. 然后进入Xcode5，创建新的Project，在"Mac OS X"里选"Other"类型，选择"External Build System"
3. 填写Product Name，我写的TestPython，然后"Build Tool"里粘贴之前在找到的Python的路径
4. 下一步，批改Scheme(which in the menu bar, select "Product" -> "Scheme" -> "Edit Scheme")  
	
	**Info选项中**
	* Executable，将None改为同样的Python路径
	* Debugger 改为None  
	
	**Arguments选项中**
	* Arguments passed on launch里写入Python文件名，比如test.py  
	
	**Options选项中**
	* Working Directory设置成存放Python文件的目录
5. 然后就可以在这个Project里新建你的test.py文件，coding， `⌘+R`然后就可以运行了

当然个人认为在Xcode中用这种方法做IDE还是相当不好用的。。。
觉得IDE非常有必要的，建议还是考虑[PyDev][3]或者[PyCharm][4]吧。。

[1]:http://stackoverflow.com/questions/5276967/python-in-xcode-4-or-xcode-5
[2]:http://www.iterm2.com
[3]:http://www.pydev.org
[4]:http://www.jetbrains.com/pycharm/
