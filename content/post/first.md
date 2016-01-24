+++
date = "2016-01-24T15:39:04+08:00"
comments = true
draft = false
share = false
image = "/images/cover.jpg"
menu = "main"
tags = ["tag1", "tag2"]
title = "first"

+++



# 提高 LaTeX 的水平
参考自知乎答案「[如何提高使用 LaTeX 的水平](http://www.zhihu.com/question/25033797/answer/29962700)」

来自**孟晨**的答案中主要推荐很多 texdoc，并且大致按照难度进行排序：

* texdoc latex2e: 这个是面向 ltx 用户的文档，介绍了 ltx 使用的方方面面。
* texdoc clsguide: 这个是面向宏包和文档类作者的，介绍了 ltx2e 的宏包和文档类的写作规范。
* texdoc doc, texdoc docstrip: 这个是「文学编程」的相关资料，看完它基本就能理解 .dtx 和 .ins 文件是怎么一回事了。
* texdoc texbytopic: 这个是我看不下去 The TeXbook 读的，二者结合起来读，能够相对轻松地了解 plainTeX 和 TeX primitive 的内容。（这两本读完之后，诸如 \expandafter 之类的命令就不会觉得晕了）
* texdoc source2e: 这个是 ltx2e 的具体实现，以及相关讲解。
* texdoc macros2e: 这个集中介绍了 ltx2e 里使用的一些内部宏，至此基本上就能写出 ltx2e 的宏包和文档类了。
* texdoc expl3: expl3 这个宏包是在 ltx2e 上实现 ltx3 的功能的一个接口，引入这个宏包就可以使用 ltx3 风格的语法。因此写 ltx3 的宏包肯定是要读它的。
* texdoc interface3: 这个是关于 ltx3 的接口的文档，我觉得像一个字典一样的东西，可以和 expl3 结合起来读。


对于**李阿玲**推荐的则主要是从简到难的内部命令，如 `\expandafter` 等。

学习基础内容从 `texdoc latex2e` 开始，然后通过 `/texlive/2014/doc.html` 这个所有地方查看所有宏包和基本介绍，了解一些需要使用的宏包。

底层内容则从 `texdoc impatient` 和 `texdoc texbytopic` 看起。

更多的关于TeX和LaTeX以及底层排版的书籍，可以到 [Books about TeX and Friends](http://tug.org/books/) 查找。
