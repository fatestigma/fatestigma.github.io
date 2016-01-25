+++
date = "2016-01-25T21:06:33+08:00"
title = "从 Octopress 迁移到 Hugo"
comments = true
draft = false
image = ""
share = true
tags = ["AppleScript", "Mac"]

+++

说到静态编辑器，首先能想到的肯定就是排名最靠前的 Jekyll, Octopress, Hexo 了[^1]。然而 Jekyll 和 Octopress 的速度一直是饱受诟病，虽然我日志数量距离三位数都还远着，先前的那个博客生成速度还远没有感受到限制，但是俗话说得好「工欲善其事必先利其器」；至于 Hexo，只能说对 NodeJS 是啥都毫无概念，直接过滤掉，再加上 Hugo 的作者是关注已久的 spf13 大神。

Hugo 最大的优点就是快，此外它对环境没有要求，直接采用编译后的二进制文件发布，所以连 Go 都不需要安装就可以直接上手撸 Hugo 了，在 Mac 上直接用 Homebrew 安装，不知道 Homebrew 的[请狗带](http://brew.sh/)；如果用的不是 Mac，也[请狗带](https://gohugo.io/overview/installing/)。

```sh
brew update && brew install hugo
```

## Hugo 简单建站步骤
首先创建你想要存放博客的地点，然后用 `hugo new site` 来建站。

```sh
hugo new site Blog
```

就这样你就建好了你自己的博客，其文件框架为
```sh
archetypes/
content/
data/
layouts/
static/
config.toml
```

其中所有所有的日志等内容应存放于 `content` 中，Hugo 会对这个目录下的[所有格式符合的文档][2]结合 `layouts` 或主题中的模板来渲染，而 `static` 中的文件会直接拷贝到最终发布文件夹 `public` 里，`data` 文件夹中[存放各种数据][3]，数据格式只能是 `.toml`, `.yaml`, `.json`。

## 戛然而止
本来打算继续按说明文的方式写下去，但是感觉这样写也未免太没有意思了，反正一开始我就是按照 [Hugo 的教程][4] 走的，按照我走的顺序翻译一遍真的一点营养都没有，因此如果这篇文章真有人看，不如直接按照教程走，我在这稍微补充一些干货好了。

- **先挑模板再设置**
	
	除非你是大神，打算从头自己无脑直接撸一个自己的网站出来以外，由于 Hugo 每个主题都是各个第三方作者制作贡献的，配置方法就很难做到协调统一，因此建议先从官方给出的 [主题库][5] 中先挑选出心仪主题，然后在根据主题内容慢慢调教。

- **自动化**

	Hugo 提供了一个非常简易的在 Github Pages 自动化部署的方法，其中主要利用 Wercker[^6] 来监控 Github 的 Source files 的提交，然后自动触发 Hugo-Build 这个动作，然后自动将渲染的网页部署，一下子将减少了很多重复性操作（既然不用自己 Build 了，应该本机上连 Hugo 都可以不用安装了吧）。

[^1]: 截止发稿当天，静态网站生成器以 GitHub Star 数量排序，前五名分别为 Jekyll, Octopress, Hexo, Hugo, Pelican。数据来源：[Static Site Generators](https://staticsitegenerators.net)
[2]: https://gohugo.io/content/supported-formats/ "Hugo Supported Files"
[3]: https://gohugo.io/extras/datafiles/ "Hugo Data files"
[4]: https://gohugo.io/overview/quickstart/ "Hugo Quickstart"
[5]: http://themes.gohugo.io/ "Hugo Themes"
[^6]: 一个持续性代码部署云服务 http://wercker.com/ 


