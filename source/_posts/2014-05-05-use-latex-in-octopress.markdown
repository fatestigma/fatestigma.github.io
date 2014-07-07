---
layout: post
title: "在Octopress中使用LaTeX"
date: 2014-05-05 13:19:05 +0800
comments: true
categories: Octopress
tags: [Octopress, LaTeX]
---

最近在看$\LaTeX$，所以突然兴起，想看看能不能在被称为A Blogging Framework for Hacker的Octopress上使用$\LaTeX$。。。  
虽然我感觉一个学计算机的在博客上用$\LaTeX$的地方应该不会很多  
但是有一颗穷折腾的心的我决定还是玩一玩。。  
在百度一番后，发现支持$\LaTeX$原来这么简单  

###步骤：<!--more-->
1. 安装Kramdown，顺便吐槽一句，Kram不是就Mark倒过来了嘛。。。- =  
{% codeblock lang:bash %}
$ gem install kramdown
{% endcodeblock %}

2. 修改`_config.yml`，将`markdown: rdiscount`改成`markdown: kramdown`，当然至于kramdown是什么我也不太清楚
3. 把下面的代码添加到`/source/_includes/custom/head.html`里
{% codeblock lang:html %}
<!-- MathJax -->
<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    tex2jax: {
      inlineMath: [ ['$','$'], ["\\(","\\)"] ],
      processEscapes: true
    }
  });
</script>

<script type="text/x-mathjax-config">
    MathJax.Hub.Config({
      tex2jax: {
        skipTags: ['script', 'noscript', 'style', 'textarea', 'pre', 'code']
      }
    });
</script>

<script type="text/x-mathjax-config">
    MathJax.Hub.Queue(function() {
        var all = MathJax.Hub.getAllJax(), i;
        for(i=0; i < all.length; i += 1) {
            all[i].SourceElement().parentNode.className += ' has-jax';
        }
    });
</script>

<script type="text/javascript"
   src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>
{% endcodeblock %}

然后就可以开始了。
只需要吧代码放在两个`$`之间就可以，比如$\LaTeX$就是`$\LaTeX$`  
还有很多非常屌的数学公式只要合乎$\LaTeX$的语法都没有问题，比如:

$$
\begin{align*}
  & \phi(x,y) = \phi \left(\sum_{i=1}^n x_ie_i, \sum_{j=1}^n y_je_j \right)
  = \sum_{i=1}^n \sum_{j=1}^n x_i y_j \phi(e_i, e_j) = \\
  & (x_1, \ldots, x_n) \left( \begin{array}{ccc}
      \phi(e_1, e_1) & \cdots & \phi(e_1, e_n) \\
      \vdots & \ddots & \vdots \\
      \phi(e_n, e_1) & \cdots & \phi(e_n, e_n)
    \end{array} \right)
  \left( \begin{array}{c}
      y_1 \\
      \vdots \\
      y_n
    \end{array} \right)
\end{align*}
$$

