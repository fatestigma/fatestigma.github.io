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


### 基本使用方法<!--more-->
***

{% codeblock lang:bash %}
# 新建一个博文
$ rake new_post["post title"]
# 预览，地址： http://localhost:4000/
$ rake preview
# 形成html网页到_deploy文件夹中
$ rake generate
# 发布
$ rake deploy
{% endcodeblock %}

### 安装主题
这里使用的是(至少在写本篇博文时)[WhiteLake][3]主题。
安装方法
{% codeblock lang:bash %}
$ cd octopress
$ git clone https://github.com/gehaxelt/CSS-WhiteLake.git .themes/whitelake
$ rake install['whitelake']
$ rake generate
{% endcodeblock %}

### 问题反馈
rake preview这个可以在不发布的时候对页面进行预览。
但是刚开始使用的时候，在Safari中并不能打开localhost:4000这个页面，在Chrome当中却没有问题。
这个问题在[github][7]上找到的解决的方法：
{% codeblock lang:bash %}
$ echo gem \"thin\" >> Gemfile
$ bundle install
{% endcodeblock %}

### Markdown
***
#### Markdown编辑器
Octopress创建的博文都是采用Markdown语言的文件。
Markdown的编辑器可以采用普通的TextEdit,
但是看到很多人都推荐[Mou][4]和[Byword][5](收费)。
当然目前个人还是比较喜欢[TextMate][6], TextMate是可以安装Markdown Bundles来拓展其功能
并且自定义Snippet也非常好用，对于常常要插入代码的程序猿们，写长长的codeblock和endcodeblock还是相当费事费时的。
所以我写了一个Snippet，用%%作为它的Tab Trigger

{% codeblock %}
{% raw %}
{% codeblock lang:${1|applescript,bash,c,cpp,css,html,java,js,jsp,matlab,objc,perl,php,py,rb,xml|} %}
${0:/* code here */}
{% endcodeblock %}
{% endraw %}
{% endcodeblock %}

这样每次要输入一个代码框，只要简单的输入%%加上一个Tab就自动输入完毕，
并自动到对应的位置选择语言。

#### Markdown介绍
由于将Markdown的解释器更换成了[Kramdown]，
所以语法和Markdown会稍有不同，Kramdown的使用可以看[Quick Reference]。

##### 换行
Markdown的换行和$\LaTeX$语言类似，都是通过一个空行来换行，
也可以通过在上一行的最后加上两个空格来起到换行的作用，
当然这两种方式在$\LaTeX$中的换行方式是不同的，
在Markdown中目前感觉没什么差别。

##### 标题
通过控制标题前的`#`的数量，来分别对应html标签的h1-h6，故最多也只有6个`#`

	Setext-style:
	This is H1
	==========
	
	This is H2
	----------
	atx-style:
	# This is H1
	## This is H2
	### This is H3
	#### This is H4
	##### This is H5
	###### This is H6

##### 文字样式
加粗：

	**被加粗的内容** or __被加粗的内容__


斜体：

	*斜体文字* or _斜体文字_


删除线：

	~~被删除文字~~

##### 引用
在行前使用`>`符号表示：

	>这是个引用哦

效果：

>这是个引用哦

引用过程可以每一行都加`>`也可以只在第一行上加，当然中间如果出现空行就断开了，还有就是可以引用嵌套以及引用中使用其他样式之类的。

##### 链接
E-mail链接和网页链接可以使用<>

	# 邮箱
	<fate_stigma@hotmail.com>
	# 链接
	1. <fatestigma.github.io>
	2. [Gallifrey][atestigma.github.io]
	3. [Gallifrey](atestigma.github.io "My Blog")	# with some description
	4. [Gallifrey][1]	# reference style
	[1]:fatestigma.github.io
	5. [Gallifrey]		# reference style
	[Gallifrey]:fatestigma.github.io

##### 列表
有序列表，注意小数点和内容间有个空格，
并且序号会由Markdown自动生成，所以自己写序号只要是数字就可以，
不一定要准确。  

	1. Ordered list item
	2. Ordered list item
	3. Ordered list item

如果恰好符合这个规范，但又不希望被转换的，可以使用`\`符号对`.`转义。

无序的可以选择`*`或者`-`号开头，Kramdown还可以选择`+`开头，还是要空一格空格。

	* Unordered list item
	* Unordered list item
	* Unordered list item 
	
	- Unordered list item
	- Unordered list item
	- Unordered list item

##### 图片
图片显示有两种方式，Markdown的还有Octopress的plugin——`image_tag`。

	![描述](path or url here "title")
	{% raw %}{% img [class name(s)] [http[s]:/]/path/to/image [width [height]] [title text | "title text" ["alt text"]] %}
	{% endraw %}


##### 表格
表格其实写起来挺麻烦的，方法如下，第二种方法添加了对**对齐方式**的声明。

	First Header | Second Header | Third Header
	------------ | ------------- | ------------
	Content Cell | Content Cell  | Content Cell
	Content Cell | Content Cell  | Content Cell
	
	First Header | Second Header | Third Header
	:----------- | :-----------: | -----------:
	Left         | Center        | Right
	Left         | Center        | Right

##### 分割线
在一行中有3个或更多的`*`或`-`就可以，但不能交替，中间可以有空格。

	***
	---
	- - - -


##### 注脚

	Some text with a footnote.[^1]
	
	[^1]: Content of this footnote

#### Octopress Plugin

##### Codeblock

	{% raw %}{% codeblock [lang:language] [title] [url] [link text] [start:#] [mark:#,#-#] [linenos:false] %}
	code snippet
	{% endcodeblock %}{% endraw %}

##### Include Code
显示文件代码，可以指定显示的行数，在`_config.yml`文件中可以指定`code_dir`来选择存放地址，
默认地址是`source/downloads/code`。

	{% raw %}{% include_code [title] [lang:language] path/to/file [start:#] [end:#] [range:#-#] [mark:#,#-#] [linenos:false] %}{% endraw %}

##### Gist Tag

	{% raw %}{% gist gist_id [filename] %}{% endraw %}

{% gist 4321346 %}

##### Video Tag

	{% raw %}{% video url/to/video [width height] [url/to/poster] %}{% endraw %}

##### jsFiddle
一个在线JS代码调试工具，这个东西之前完全没有用过。还是先把语法放在这。

	{% raw %}{% jsfiddle shorttag [tabs] [skin] [height] [width] %}{% endraw %}

在放一个演示：

{% jsfiddle ccWP7 %}

<br />

##### Image Tag
也是一个插入图片的工具。

	{% raw %}{% img [class names] /path/to/image [width] [height] [title text [alt text]] %}{% endraw %}

##### Blockquote
相当于Markdown的`>`

	{% raw %}{% blockquote [author[, source]] [link] [source_link_title] %}
	Quote string
	{% endblockquote %}{% endraw %}
	
##### Pullquote
这个样式还是挺好的，在Markdown中也没有找到对应替代的，将Quote放在右侧

	{% raw %}{% pullquote %} or {% pullquote left %}
	Surround your paragraph with the pull quote tags. Then when you come to the text you want to pull, {" surround it like this "} and that's all there is to it.
	{% endpullquote %}{% endraw %}

{% pullquote %}
Left-aligning pullquotes are good to alternate breaks in the text. They're
{" almost exactly like the default, "} right pullquotes, but a little different.
{% endpullquote %}

##### Render Partial
在当前Markdown文件中的当前位置添加新的Markdown文件，
相当于其他语言的`include`语句或者说是$\LaTeX$的`\input`。

	{% raw %}{% render_partial path/to/file %}{% endraw %}

##### 更多
还有很多插件，都可以在`plugins`文件夹下查看。


[Kramdown]:http://kramdown.gettalong.org
[Quick Reference]:http://kramdown.gettalong.org/quickref.htm
[1]:http://biaobiaoqi.me/blog/2013/03/21/building-octopress-in-github-mac/
[2]:http://octopress.org/docs/setup/
[3]:https://github.com/gehaxelt/CSS-WhiteLake
[4]:http://mouapp.com
[5]:http://bywordapp.com
[6]:http://macromates.com
[7]:https://github.com/imathis/octopress/issues/1395
