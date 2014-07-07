---
layout: post
title: "Alfred 2 Workflows: TV show helper"
date: 2014-06-28 16:42:36 +0800
comments: true
categories: Workflows
tags: Alfred PHP
---

对于一个喜欢看美剧和动漫的人来说，最头疼的就是看片过多导致每部都不记得看到哪了，
我早期选择的使用Numbers作一个表格，每看完一集就在表格里修改一次，
但是这个方法确实是相当麻烦。特此借助[Alfred 2][1]制作了一个Workflow来快速查看并修改。

**使用方法:**

>* `watch {show}` 快速查看{show}的当前进度  
* `watch undo` 撤销上一次修改，包括init、finish  
* `watch all` 查看当前所有片的进度  
* `finish {show}` 看完，从表中删除  
* `watch init {eps} {show}` 直接修改{show}的当前进度为{eps}  

<!--more-->

此Workflow是用PHP写的，个人对PHP实在是了解甚少，所以一直是边写边查文档，
很多功能的实现可能麻烦很多。

![image](https://github.com/fatestigma/AlfredWorkflows/raw/master/extra/show.png)

**[点此下载][2]**  
**[GitHub][3]**

[1]:http://www.alfredapp.com
[2]:https://github.com/fatestigma/AlfredWorkflows/blob/master/Downloads/TV%20Show%20helper%20v0.3.alfredworkflow
[3]:https://github.com/fatestigma/AlfredWorkflows


