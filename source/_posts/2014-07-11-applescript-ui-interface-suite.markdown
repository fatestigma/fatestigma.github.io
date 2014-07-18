---
layout: post
title: "AppleScript的UI套件"
date: 2014-07-11 23:52:28 +0800
comments: true
categories: AppleScript
tags: AppleScript
---

>最近一直沉迷于Alfred的Workflows，所以对AppleScript开始有所研究。
AppleScript作为和Automator搭配的语言，对于控制Mac OS X系统自然是非常方便，
而且Apple对这个语言也一直在更新，众多Mac的原生应用如iTerm、OmniFocus、
还有本家的iWork套件都有对它的支持，其中可以在AppleScript Editor中的应用字典(⇧⌘O)中查询。

AppleScript的相关参考书籍并不是很多，所以大部分只能靠百度（无奈谷歌在我朝已亡），
正好最近写一个Workflows时学到很多关于Dialog等UI的应用，特记录一下方便以后查看。

所有的UI相关内容都在AppleScript的User Interface suite中，其中包括有：
[beep]、[choose application]、[choose color]、[choose file]、
[choose file name]、[choose folder]、[choose folder list]、
[choose remote application]、[choose URL]、[delay]、[display alert]、
[display dialog]、[display notification]、[say]。
他们的功能其实只要看名称就能知道了，这也是AppleScript的特征，具体使用方法可以直接点开链接，
下面我主要介绍的是**display dialog**和**display notification**这两个：
<!--more-->

##### display dialog

| display dialog | text | 显示的内容 |
| default answer | text | 默认回答，不选则为空 |
| hidden answer | boolean | 隐藏输入，用于密码输入 |
| buttons | list | 自定义按钮，至多3个 |
| default button | labelSpecifier | 确认按钮(高亮)，接受Enter键 |
| cancel button | labelSpecifier | 取消按钮，接受Escape键 |
| with title | text | 窗口的标题 |
| with icon | resourceSpecifier | 左侧显示的图标 |
| with icon | iconTypeSpecifier |
| with icon | fileSpecifier|
| giving up after | integer | 等待输入的时间 |

  
除了第一个是必须的，其他都是可选的，
值得注意的是buttons只能显示3个按钮，它的值是list of text；
而with icon的后面可接受的参数有3种resourceSpecifier(text | integer)、
iconTypeSpecifier(stop |note | caution)、fileSpecifier(alias |file)，
只有最后一个是自定义的图片的方式。

{% codeblock lang:applescript %}
display dialog "请输入密码" hidden answer true buttons {"男", "女"} with icon caution
{% endcodeblock %}

返回内容对于正确输入{text returned:"password", button returned:"男"}，  
对于按下取消键或者被cancel button标注的，则会报错"user canceled"，
程序终止或用try-catch来处理异常，  
如果添加了giving up after，则会返回{button returned:"", gave up:true/false}

##### display notification

| display notification | text | required
| with title | text | optional
| subtitle | text | optional
| sound name | text | optional


和上一个类似，也是只有第一个是必需项，后面添加标题、副标题和提示声便可，
提示声储存在`~/Library/Sounds`中。

{% codeblock lang:applescript %}
display notification "Message" with title "I'm titled" subtitle "subtitle wow" sound name "1.wav"
{% endcodeblock %}
效果如图，并播放`~/Library/Sounds`中名位`1.wav`的音频。
{%img /images/post/140713/notification-ex1.png %}

更多内容可以参考官方文档[Commands Reference]

以下是Mac下的快捷键图案，先放这备份。

> ⌘——Command ()  
> ⌃ ——Control  
> ⌥——Option (alt)  
> ⇧——Shift  
> ⇪——Caps Lock  
> ⏏——Eject  
> ⇥——Tab  
> ⎋——Escape  
> ⌫——Delete  
> ␣——Space  


[beep]:https://developer.apple.com/library/mac/documentation/applescript/conceptual/applescriptlangguide/reference/aslr_cmds.html#//apple_ref/doc/uid/TP40000983-CH216-SW1
[choose application]:https://developer.apple.com/library/mac/documentation/applescript/conceptual/applescriptlangguide/reference/aslr_cmds.html#//apple_ref/doc/uid/TP40000983-CH216-SW2
[choose color]:https://developer.apple.com/library/mac/documentation/applescript/conceptual/applescriptlangguide/reference/aslr_cmds.html#//apple_ref/doc/uid/TP40000983-CH216-SW3
[choose file]:https://developer.apple.com/library/mac/documentation/applescript/conceptual/applescriptlangguide/reference/aslr_cmds.html#//apple_ref/doc/uid/TP40000983-CH216-SW4
[choose file name]:https://developer.apple.com/library/mac/documentation/applescript/conceptual/applescriptlangguide/reference/aslr_cmds.html#//apple_ref/doc/uid/TP40000983-CH216-SW5
[choose folder]:https://developer.apple.com/library/mac/documentation/applescript/conceptual/applescriptlangguide/reference/aslr_cmds.html#//apple_ref/doc/uid/TP40000983-CH216-SW6
[choose folder list]:https://developer.apple.com/library/mac/documentation/applescript/conceptual/applescriptlangguide/reference/aslr_cmds.html#//apple_ref/doc/uid/TP40000983-CH216-SW7
[choose remote application]:https://developer.apple.com/library/mac/documentation/applescript/conceptual/applescriptlangguide/reference/aslr_cmds.html#//apple_ref/doc/uid/TP40000983-CH216-SW8
[choose URL]:https://developer.apple.com/library/mac/documentation/applescript/conceptual/applescriptlangguide/reference/aslr_cmds.html#//apple_ref/doc/uid/TP40000983-CH216-SW9
[delay]:https://developer.apple.com/library/mac/documentation/applescript/conceptual/applescriptlangguide/reference/aslr_cmds.html#//apple_ref/doc/uid/TP40000983-CH216-SW10
[display alert]:https://developer.apple.com/library/mac/documentation/applescript/conceptual/applescriptlangguide/reference/aslr_cmds.html#//apple_ref/doc/uid/TP40000983-CH216-SW11
[display dialog]:https://developer.apple.com/library/mac/documentation/applescript/conceptual/applescriptlangguide/reference/aslr_cmds.html#//apple_ref/doc/uid/TP40000983-CH216-SW12
[display notification]:https://developer.apple.com/library/mac/documentation/applescript/conceptual/applescriptlangguide/reference/aslr_cmds.html#//apple_ref/doc/uid/TP40000983-CH216-SW13
[say]:https://developer.apple.com/library/mac/documentation/applescript/conceptual/applescriptlangguide/reference/aslr_cmds.html#//apple_ref/doc/uid/TP40000983-CH216-SW14
[Commands Reference]:https://developer.apple.com/library/mac/documentation/applescript/conceptual/applescriptlangguide/reference/aslr_cmds.html#//apple_ref/doc/uid/TP40000983-CH216-SW12



