---
layout: post
title: "Alfred Workflows: TV show helper"
date: 2014-06-28 16:42:36 +0800
comments: true
categories: Workflows
tags: Alfred PHP
---

>对于一个喜欢看美剧和动漫的人来说，最头疼的就是看片过多导致每部都不记得看到哪了，
我早期选择的使用Numbers作一个表格，每看完一集就在表格里修改一次，
但是这个方法确实是相当麻烦。特此借助[Alfred 2]制作了一个Workflow来快速查看并修改。

**使用方法:**

>* `watch {show}` 快速查看{show}的当前进度  
* `watch undo` 撤销上一次修改，包括init、finish  
* `watch all` 查看当前所有片的进度  
* `finish {show}` 看完，从表中删除  
* `watch init {eps} {show}` 直接修改{show}的当前进度为{eps}  

<!--more-->

此Workflow是用PHP写的，个人对PHP实在是了解甚少，所以一直是边写边查文档，
很多功能的实现可能麻烦很多。

### Updated @ Aug. 27, 2014
>听取部分网友的建议，加入了**网盘同步**的功能，当然文件本身是二进制的plist文件。

同步方法：对此Workflows下的所有script和script filter中将第一行最后一个参数`nosync`修改成为你电脑本地同步的路径即可。

![image](https://github.com/fatestigma/AlfredWorkflows/raw/master/extra/show.png)

**[点此下载]**  
**[GitHub]**

[Alfred 2]:http://www.alfredapp.com
[点此下载]:https://github.com/fatestigma/AlfredWorkflows/releases/download/0.5-beta/TV.Show.helper.v0.5.beta.alfredworkflow
[GitHub]:https://github.com/fatestigma/AlfredWorkflows


