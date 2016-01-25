+++
author = ""
comments = true
date = "2014-12-01T19:28:17+08:00"
draft = false
image = ""
menu = ""
share = true
tags = ["AppleScript", "Mac"]
title = "触发角的快速切换"

+++

>最近在百度知道上给一个童鞋写了一个快速切换触发角的 AppleScript，来实现在游戏启动前运行给所有触发角添加一个修饰键（如 Command），结果发现需求的人还是挺多的。
觉得有必要写一篇简单的教程。

其实原理非常简单，就是直接用终端的 `defaults` 来修改配置文件，这里的配置文件就是：“com.apple.dock”。
当然 AppleScript 中本身具有修改 plist 文件的方法，但是这个不熟，不如直接使用终端命令来的方便。

<!-- more -->

对于每个触发角都有两个属性，分别是功能和修饰键，在 "com.apple.dock" 中表现为：`wvous-pos-corner` 和 `wvous-pos-modifier`，其中 pos 是要替换的，可以替换成 `bl`, `tl`, `br`, `tr` 四个，我认为其中 `b` 表示 bottom，`t` 表示 top，`r` 表示 right，`l` 表示 left。

下面就是代码，这里做的是无修饰键和⌘键之间的切换，
如果需要切换其他的可以直接在下面代码中修改 `adjustModifier` 的值。

{{< gist fatestigma 4678ddfc8a73720f388c >}}

## 附录
### 修饰键和键码对照

|Modifier|Keycode|
|--:|:--:|
|null|0|
|⌘|1048576|
|⌥|524288|
|⌃|262144|
|⇧|131072|
|⌘⌥|1572864|
|⌘⌃|1310720|
|⌘⇧|1179648|
|⌥⌃|786432|
|⌥⇧|655360|
|⌃⇧|393216|
|⌘⌥⌃|1835008|
|⌘⌥⇧|1703936|
|⌥⌃⇧|917504|
|⌘⌥⌃⇧|1966080|

### 触发角功能对应码


|Corner|Code|
|:--|:--:|
|null|1|
|Mission Control|2|
|Application Windows|3|
|Desktop|4|
|Dashboard|7|
|Notification Center|12|
|Launchpad|11|
|Start Screen Saver|5|
|Disable Screen Saver|6|
|Put Display to Sleep|10|

