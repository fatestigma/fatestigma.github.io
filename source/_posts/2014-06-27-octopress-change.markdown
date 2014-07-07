---
layout: post
title: "对Octopress的更改记录"
date: 2014-06-27 23:18:51 +0800
comments: true
categories: Octopress 
tags: Octopress 
---

#### 1.改用友言代替Disqus @ Jun 27th, 2014
好久没有上Octopress了。。当初花2天辛苦搭建完之后，
热情澎湃的写了5篇就由于各种原因没有再动了。。
最近刚好考完试，没有什么事情，就又上来了。。  
登陆的时候发现登陆速度完全不给力啊，尤其是Disqus的评论加载，
所以打算把它换成[友言][1]了。 
方法选用的是唐巧的[搭建基于github的博客][2]中的添加友言和加网

<!--more-->

#### 2.博客添加到GitCafe @ Jun 27th, 2014
刚好是看完唐巧的博客添加完友言评论后看到[将博客从GitHub迁移到GitCafe][3]这个文章，
确实很多时候GitHub的速度令人捉急，按照步骤通过修改Rakefile在每次deploy时候也push一份到GitCafe。  
只不过后来这个速度也不是很理想。。但是还能凑合着用。。

#### 3.去除Google Font的加载 @Jun 27th, 2014
加载速度不能再慢了，通过Safari的Web Inspector发现其中大部分时间都用来加载Google Fonts了。
众所周知最近这段时间Google是上不去的了，这个时候还一直加载Google Fonts明显作死。
因此直接将`source/_includes/custom/head.html`中linkGoogle Fonts的代码注释掉，代码如下：
{% codeblock lang:diff %}
  <!--Fonts from Google"s Web font directory at http://google.com/webfonts -->
- <link href="http://fonts.googleapis.com/css?family=PT+Serif:regular,italic,bold,bolditalic" rel="stylesheet" type="text/css">
- <link href="http://fonts.googleapis.com/css?family=PT+Sans:regular,italic,bold,bolditalic" rel="stylesheet" type="text/css">
+ <link href="/stylesheets/google-fonts.css" rel="stylesheet" type="text/css">
  <!-- mathjax config similar to math.stackexchange -->

{% endcodeblock %}

然后在`source/_includes/stylesheets/`路径下新建一个文件`google-fonts.css`如下。

{% include_code google-fonts.css %}

#### 4.修改代码缩进 @Jul 7th, 2014
今天突然发现所有代码块中间的缩进竟然都是8，对于写AppleScript这种嵌套很多的，
显示出来完全没法看了。在`sass/custom/_style.scss`中添加：
{% codeblock lang:css %}
pre {tab-size: 4;}
{% endcodeblock %}
当然也可以对不同的语言不同的方法处理，如:
{% codeblock lang:css %}
code.html {tab-size: 2;}
{% endcodeblock %}

#### 5.给codeblock添加折叠按钮 @Jul 7th, 2014
每篇文章当中如果代码写的太多，很容易让人觉得挺烦，
所以我觉得加个折叠按钮还是挺有必要的。
看了半天Octopress的源代码，终于找到了，首先添加按钮，在`plugins/code_block.rb`中
第65行和第68行中修改如下：

{% codeblock lang:rb %}
      if markup =~ CaptionUrlTitle
        @file = $1
        @caption = "<figcaption><button onclick=\"codehide(this);\">折叠代码</button><span>#{$1}</span><a href='#{$2}'>#{$3 || 'link'}</a></figcaption>"
      elsif markup =~ Caption
        @file = $1
        @caption = "<figcaption><button onclick=\"codehide(this);\">折叠代码</button><span>#{$1}</span></figcaption>\n"
      end
{% endcodeblock %}

然后是在`source/_includes/custom/head.html`中添加如下代码：
{% codeblock lang:html %}
<script type="text/javascript">
function codehide(obj) {
	var cb = obj.parentNode.nextSibling.nextSibling;
	if (cb.style.display != 'none') {
		cb.style.display = 'none';
		obj.innerHTML = '展开代码';
	} else {
		cb.style.display = 'block';
		obj.innerHTML = '折叠代码';
	}
}
</script>
<style>
button {
	border: 0;
	background: transparent;
	position: absolute;
	left: 0.8em;
}
</style>
{% endcodeblock %}

#### 6.再次改用Disqus @Jul 7th, 2014
友言的样式真的是太不搭我这个Theme了。还是Disqus的样子比较配合，
再加上最近感觉Disqus的速度又比较稳定了，所以又改了过来。
只不过至今我都添加评论系统近2个月了，还是没人评论过，各种怨念。。



[1]:http://www.uyan.cc
[2]:http://blog.devtang.com/blog/2012/02/10/setup-blog-based-on-github#code
[3]:http://blog.devtang.com/blog/2014/06/02/use-gitcafe-to-host-blog/
