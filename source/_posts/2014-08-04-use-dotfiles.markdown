---
layout: post
title: "Dotfiles来备份与同步配置文件"
date: 2014-08-02 01:38:25 +0800
comments: true
categories: Mac 
tags: dotfiles
---

最近一段时间由于手贱玩Yosemite，重装了次系统后发现,应用什么的很容易就找回来了，但是对于各种配置文件来说就没有那么容易了，以前的`.bash_profile`,`.vimrc`等等又要重新配置重新安装各种插件了，真心很是麻烦。后来发现有个叫[dotfiles]的项目，旨在将这些配置文件集中到一起惊醒版本管理和云储存，然后在新的电脑中连接这些文件，使得他们和你原来电脑使用起来基本一样。

其最为明显的优点有：

1. 可以备份的同时，在多台电脑间同步你的设置
2. 将同一个应用的所有配置文件放在一起，可以很方便的进行版本控制
3. 方便分享

<!-- more -->

## 同步与使用
大部分的dotfiles使用者都会选择[DropBox]来作为他们同步软件，但是由于国情，很明显这对于我们并不是一个很好的选择。我个人以前非常偏好使用金山快盘，但是金山快盘不会同步`.`开头的文件，所以弃用了，经过各种测试，发现[百度云]好像是支持`.`开头文件的同步。所以目前就先用[百度云]了；当然不仅仅是使用同步工具来同步，另外还使用git来对我的配置文件进行集中地版本控制，最后可以放在GitHub上与他人分享。

首先说下我的总文件夹名称和大家选用一样的`.dotfiles`（我想当初叫这个名字主要也是大部分配置文件都是以点开头的），然后将`.dotfiles`放在云盘中，并采用软连接放入`~/`中，最后再通过各种软连接来将里面的各个配置文件放到原来的位置。

## 内容
首先将各个配置文件归类放在`.dotfiles`文件夹下，

	.dotfiles
	├── .git
	├── .gitignore
	├── install.sh	# 对所有配置文件的软连接
	├── README.md
	├── config		# 各种小型的配置文件
		├── gitconfig
		├── gitignore_global
		└── ssh
	├── osx			# 放置对Mac电脑的偏好设置修改文件
		└── osx_config.sh
	├── script 		# 放置几个安装脚本
		├── homebrew_install.sh
		└── python_install.sh
	├── vim			# 放置我的`.vimrc`等和Vundle的bundle
		├── vimrc
		└── bundle
	└── zsh			# 放置zsh和oh-my-zsh的配置文件
		└── zshrc

将各个配置文件归类后，并且添加一个`install.sh`来将所有的配置文件软连接归为：
{% codeblock lang:bash %}
# vim
ln -s ~/.dotfiles/vim ~/.vim
ln -s ~/.dotfiles/vim/vimrc ~/.vimrc

# for oh-my-zsh
ln -s ~/.dotfiles/zsh/zshrc ~/.zshrc

# for config files
ln -s ~/.dotfiles/config/gitconfig ~/.gitconfig
ln -s ~/.dotfiles/config/gitignore_global ~/.gitignore_global
ln -s ~/.dotfiles/config/ssh ~/.ssh
{% endcodeblock %}

另外通过使用`osx_config.sh`来配置一台新的Mac的偏好设置，通过`homebrew_install.sh`来安装之前的电脑brew过的应用，通过`python_install.sh`来安装之前安装过的库，同时也可以看出这三个文件需要定时的管理和维护。

`.gitignore`文件中主要记录不想被同步到GitHub上的内容，其中主要就是`ssh`文件夹下的内容了。
{% codeblock %}
# privacy in ssh
config/ssh
{% endcodeblock %}


[dotfiles]:http://dotfiles.github.io
[DropBox]:www.dropbox.com
[百度云]:http://pan.baidu.com/

