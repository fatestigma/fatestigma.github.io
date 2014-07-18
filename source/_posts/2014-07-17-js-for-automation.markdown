---
layout: post
title: "JavaScript for Automation翻译"
date: 2014-07-17 23:49:49 +0800
comments: true
categories: 
tags: 
---

>JavaScript for Automation是Yosemite的新功能之一，
或许在Keynote上并没有讲到，但是在WWDC讲座306中确有详细的介绍。
在苹果公司推出自家新语言Swift的同时，
却又做出这种疑似抛弃自家脚本语言AppleScript的举动，
当然这个举动对于大部分开发者来说是好事，
可以少学一门新语言并用一个自己更为熟悉的语言来代替。

那么接下来我主要就根据WWDC的306讲座内容和[Pre-Release]的内容简单介绍一下。

首先是原本的AppleScript Editor已经正式改名叫Script Editor了，
这个其实在iCloud上面已经体现出来了，
界面布局和以前的基本一样，当然风格是换成Yosemite的风格了。

## 获取应用的方法
首先是获取一个应用的接口方法，
在AppleScript中是采用application "Finder"这种方法，
而在JavaScript中提供更多的方法，如：
|Name				|Application('Finder')|
|Bundle ID			|Application('com.apple.finder')|
|Path				|Application('/System/Library/CoreServices/Finder.app')|
|Process ID			|Application(206)|
|Current Application|Application.currentApplication()|
靠Name获取和AppleScript类似，最后一个直接获取当前应用应该是说挺有用的，
其他的感觉略鸡肋，当然对于同名应用可能还是挺有用的吧。

<!--more-->

## 语法示例

1. 属性获取

	Finder.name
2. 元素获取

	Finder.document[0]
3. 命令调用

	Finder.open(...)
4. 创建类实例

	Finder.Document(...)

## 属性获取与设置
url = Application('Safari').document[0].url()
url = 'http://localhost'
这个就相当于AppleScript的set...to...语句，
只不过去掉了大量的语法糖，但语言对于程序员们来说依然很好阅读。  

1. Index

	window = Mail.windows[0]

2. Name

	window = Mail.windows['New Message']

3. ID

	window = Mail.windows['#412']

## 条件过滤
对数组(arrays)通过`whose`指令来使用条件过滤，
获得的便是原数组中满足条件的元素组合成的一个子集。

	jsEmails = Application('Mail').inbox.messages.whose({subject:'JavaScript'})

## 函数调用

1. 无参函数
	Application('Mail').inbox.messages[0].open()
2. 含参函数
	Application('Mail').open(messages)
	Application('Mail').inbox.messages[0].reply)({
		replyAll: true,
		openingWindow: false
	})


虽然语法上不再像AppleScript那样符合人类语言，
但其实可读性也是很好的。









[Pre-Release]:https://developer.apple.com/library/prerelease/mac/releasenotes/InterapplicationCommunication/RN-JavaScriptForAutomation/index.html#//apple_ref/doc/uid/TP40014508
