---
layout: post
title: "开始使用Octopress做我的博客"
date: 2014-05-01 01:49:49 +0800
comments: true
categories: Octopress
tags: Octopress
---

整了大半夜终于把这个Octopress搭建好了，其实很久之前也有搭建Octopress的想法，只不过总是在Ruby的版本上卡壳(好吧，其实是我强迫症，推荐使用Ruby1.9.3的时候我非要装个2.0)。

但是今天抱着试一试的想法，打算不成功便用Jekyll算了，结果没想到竟然成功了。

当然这里还是非常感谢Biaobiaoqi的博文[在github上搭建octopress博客 Mac版][1]和官方的文档[Octopress Setup][2]的帮助。


###基本使用方法<!--more-->
***

{% codeblock lang:bash %}
#### 新建一个博文
$ rake new_post["post title"]
#### 预览，地址： http://localhost:4000/
$ rake preview
#### 形成html网页到_deploy文件夹中
$ rake generate
#### 发布
$ rake deploy
{% endcodeblock %}

###安装主题
这里使用的是(至少在写本篇博文时)[WhiteLake][3]主题。
安装方法
{% codeblock lang:bash %}
$ cd octopress
$ git clone https://github.com/gehaxelt/CSS-WhiteLake.git .themes/whitelake
$ rake install['whitelake']
$ rake generate
{% endcodeblock %}

###Markdown
Octopress创建的博文都是采用Markdown语言的文件。
Markdown的编辑器可以采用普通的TextEdit,
但是看到很多人都推荐[Mou][4]和[Byword][5](收费)。
当然目前个人还是比较喜欢[TextMate][6], TextMate是可以安装Markdown Bundles来拓展其功能
并且自定义Snippet也非常好用，对于常常要插入代码的程序猿们，写长长的codeblock和endcodeblock还是相当费事费时的。
所以我写了一个Snippet，用%%作为它的Tab Trigger

{% codeblock %}
{ % codeblock lang:${1|applescript,bash,c,cpp,css,html,java,js,jsp,matlab,objc,perl,php,py,rb,xml|} % }
${0:/* code here */}
{ % endcodeblock % }
{% endcodeblock %}

###问题反馈
rake preview这个可以在不发布的时候对页面进行预览。
但是刚开始使用的时候，在Safari中并不能打开localhost:4000这个页面，在Chrome当中却没有问题。
这个问题在[github][7]上找到的解决的方法：
{% codeblock lang:bash %}
$ echo gem \"thin\" >> Gemfile
$ bundle install
{% endcodeblock %}


[1]:http://biaobiaoqi.me/blog/2013/03/21/building-octopress-in-github-mac/
[2]:http://octopress.org/docs/setup/
[3]:https://github.com/gehaxelt/CSS-WhiteLake
[4]:http://mouapp.com
[5]:http://bywordapp.com
[6]:http://macromates.com
[7]:https://github.com/imathis/octopress/issues/1395
