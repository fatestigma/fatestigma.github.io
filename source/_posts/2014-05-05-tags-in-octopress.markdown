---
layout: post
title: "在Octopress中添加Tags"
date: 2014-05-05 14:13:02 +0800
comments: true
categories: Octopress
tags: Octopress
---

Octopress 默认情况下只有分类(Category)，没有标签，看现在的大部分blog基本都有Tags，所以决定也弄一个好了。  
首先步骤是按照Kevin Lynx的[为octopress添加tag Cloud][1]制作的  

###步骤：<!--more-->
1. Clone [Tag Pages][2]和[Tag Cloud][3]这两个项目的代码，两个代码的作用看名字也基本清楚了。  
2.  
	**Octopress-Tag-Pages**
	* 复制tag_generator.rb到/plugins目录；复制tag_index.html到/source/_layouts目录。需要注意的是，还需要复制tag_feed.xml到/source/_includes/custom/目录。这个官方文档里没提到，在我机器上rake generate时报错。其他文件就不需要复制了，都是些例子。
	**Octopress-Tag-Cloud**
	* 仅复制tag_cloud.rb到/plugins目录即可。但这仅仅只是为liquid添加了一个tag（非本文所提tag）。如果要在侧边导航里添加一个tag cloud，我们还需要手动添加aside。
3. 
	复制以下代码到`/source/_includes/custom/asides/tags.html`
{% codeblock lang:html %}
<section>
  <h1>Tags</h1>
  <ul class="tag-cloud">
	# tag_cloud font-size: 90-210%, limit: 10, style: para #
  </ul>
</section>
{% endcodeblock %}  
tag_cloud的参数中，style :para指定不使用li来分割，limit限定10个tag，font-size指定tag的大小范围，具体参数参看官方文档。

在`_config.xml`的default_asides 中添加这个tag cloud到导航栏
{% codeblock lang:xml %}
default_asides: [..., custom/asides/tags.html]
{% endcodeblock %}

当然还有一些Kevin Lynx没有讲到的，比如修改`Rakefile`，让`rake new_post`可以自动生成tags  
即在`Rakefile`大概118行中添加
{% codeblock lang:rb %}
    post.puts "tags: "
{% endcodeblock %}

###Bug：
在我添加完tags后，发现一个更严重的问题，就是在我添加超过一个tags之后，`rake generate`就会开始报错了：
`Error :Liquid Exception: comparison of Array with Array failed in page`
后来在Tag Cloud 的[Issue][4]中找到了解决办法
{% codeblock lang:diff %}
@@ -66,7 +66,11 @@ module Jekyll
       # map: [[tag name, tag count]] -> [[tag name, tag weight]]
       weighted = count.map do |name, count|
         # logarithmic distribution
-        weight = (Math.log(count) - Math.log(min))/(Math.log(max) - Math.log(mi
+        if min == max   #ADDED MYSELF
+          weight = 1    #ADDED MYSELF
+        else            #ADDED MYSELF
+          weight = (Math.log(count) - Math.log(min))/(Math.log(max) - Math.log(
+        end             #ADDED MYSELF
         [name, weight]
       end
{% endcodeblock %}


[1]: http://codemacro.com/2012/07/18/add-tag-to-octopress/
[2]: https://github.com/robbyedwards/octopress-tag-pages
[3]: https://github.com/robbyedwards/octopress-tag-cloud
[4]: https://github.com/robbyedwards/octopress-tag-cloud/issues/1
