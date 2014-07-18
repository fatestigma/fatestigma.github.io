---
layout: post
title: "使用Alfred来简化发博"
date: 2014-05-01 22:19:44 +0800
comments: true
categories: Workflows
tags: Alfred
---

进过昨晚一夜的奋斗，终于把我的Octopress搭建完成。  
但是这种每次发布和预览都需要用终端命令输入一堆东西，尤其还要从终端到Octopress的文件夹下，像我这么懒的人还真是受不了。  
而这些天正好在研究[Alfred][1]的Workflows，非常神奇、高效的一个应用，配合上自定义的Workflows完全就是神器了。(当然Workflows的部分功能用Automator和AppleScript配合也能完成)  
所以自然而然想到能不能用Alfred指令来快速完成这些繁琐的过程。
<!--more-->

当然刚开始我是打算做几个bash文件放在`$HOME`里，但是bash文件运行起来还是挺麻烦的(我就是这么懒)。。  
还是贴出来看看好了
{% codeblock lang:bash %}
#!/bin/bash
# this script used to generate my octopress pages and directly deploy it on github
# use `bash ./post.sh` to run it

cd ~/Git/octopress/;
rake generate deploy
{% endcodeblock %}

### 接下来开始正题
{%img /images/post/140501/all.png %}

首先是分成3条指令post、preview和new   
第一条post用于快速将他们全部提交上去

当然我平时主要更常用"iTerm"，所以写的是用[iTerm][2]的。。

{% codeblock lang:applescript %}
tell application "iTerm"
	activate
	set _term to (make new terminal)
	tell _term
		launch session "Default"
		set _session to current session
	end tell
	
	tell _session
		write text "cd ~/Git/octopress/"
		write text "rake generate deploy"
	end tell
end tell
{% endcodeblock %}

对于preview只用把上段代码的第11行修改就可以了：

{% codeblock lang:applescript %}
write text "rake generate preview"
{% endcodeblock %}

但是对于preview不能就这样完了，可以在上图中看到在preview的第一个"Run Script"后面还链了一个"Run Script"，第二个的任务就是打开preview的网页了

{% codeblock lang:applescript %}
tell application "Safari"
	delay 3
	activate
	tell window 1
		set current tab to (make new tab with properties {URL:"http://localhost:4000"})
	end tell
end tell
{% endcodeblock %}

第二部分这个非常简单，但是用delay 3来延缓3秒钟，要不前面部分的preview还没有完成，只能打开一个无法显示。。

最后new就是新建一个，对应rake new_post["xxx"]，也是将post的代码第11行改成：
{% codeblock lang:applescript %}
write text "rake new_post[\"{query}\"]"
{% endcodeblock %}

这样就全都完成了。

{%img /images/post/140501/new_test.png %}

非常方便就建立好了一个新的Markdown。

过几天就打包分享看看有没有人用好了。。

OK...这篇blog也写完了，`⌥+space`  
{%img /images/post/140501/post_test.png %}

[1]:http://www.alfredapp.com
[2]:http://www.iterm2.com
