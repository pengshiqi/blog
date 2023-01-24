---
title: 我的MacBook
date: 2017-08-28 10:31:10
tags:
- Mac
- Linux
categories:
- 学习
---

“工欲善其事，必先利其器”，这篇就讲讲我的学习开发环境吧~


我是两年前开始用MacBook Pro的，感觉是真的好用，于是渐渐成了一个果粉，后面每当有朋友想买电脑时都会推荐MBP...


MBP 主要的优势在于 user-friendly，并且是 \*nix 系统，相比windows省去了很多配置环境上的麻烦，相比Linux又好用许多.

<!-- more -->


## 1. IDE(Integrated Development Environment)

### 1.1 Xcode 

<img src="xcode.png" style="zoom:45%" />

首先必然是OS X系统的招牌IDE：`Xcode`。

虽然平时写代码不一定用到Xcode，但是伴随着它会安装许多其他工具，比如clang, llvm等。

Xcode主要是用来开发苹果产品上的软件的，使用Swift可能会比较舒服，但我只用Xcode写过C++程序，感觉一般吧。


### 1.2 JetBrains全家桶

<!-- ![jetbrains](jetbrains.png) -->
<img src="jetbrains.png" style="zoom:100%" />


JetBrains是一家捷克的公司，他们的IDE做的非常好，有相似的UI，以及可以跨平台（Windows, Linux, OS X）使用，感觉就是IDE界的Adobe...其中PyCharm和IntelliJ IDEA有community版本，是免费的，其他的是用edu邮箱注册的学生账号使用的。

PyCharm是python开发必备，有强大的提示功能和Debug功能，以及更多我没发现的功能…唯一的弊端大概就是开启比较慢，并且开启的时候消耗内存、CPU很大。

IntelliJ IDEA已经是当前坠吼的 Java IDE了，比Eclipse不知道高到哪里去了。

CLion是用来C++开发的，之前有门课程的大作业就是用CLion写的，相比VS，它需要自己写makefile。

WebStorm是用来web开发的，DataGrip是数据库开发的，用的不多。


## 2. 编辑器(Editor)

<!-- ![editor](editor.png) -->
<img src="editor.png" style="zoom:100%" />

前三个应该是目前主流的三个编辑器了。Sublime Text是需要购买，但可以免费使用的; VS code 和 Atom 分别是微软和GitHub的产品，都是开源Hackable的编辑器。

这三个我都用过，各有好处也各有弊端。最近在用Sublime Text，因为它更新后UI变得更好看了，弃用Atom的原因主要还是它打开太慢...这或许跟我安装太多插件有关...

Typora 是mac上非常好用的一款Markdown编辑器，界面美观，写markdown首选，谁用谁知道。

Brackets 据说是一款很好用的前端开发编辑器，还没有深度使用过。

## 3. iTerm2

<!-- ![iterm2](iterm2.png) -->
<img src="iterm2.png" style="zoom:100%" />

Mac 自带的Terminal是bash，界面不太美观，于是使用 `iTerm2 + Oh My Zsh`  配上 `powerline` 高度自定义了Terminal。

<!-- ![vim](vim.gif) -->
<img src="vim.png" style="zoom:100%" />

vim 是Terminal中常用的编辑器，我在 [GitHub](https://github.com/wklken/k-vim) 上找了一个同学的配置来使用。

此外，在Terminal中还有一款非常好用的插件叫 [`Tmux`](https://github.com/tmux/tmux/wiki),

> tmux is a terminal multiplexer. It lets you switch easily between several programs in one terminal, detach them (they keep running in the background) and reattach them to a different terminal. And do a lot more. See the tmux(1) manual page and the README.

## 4. 其他软件

<!-- ![efficiency](efficiency.png) -->
<img src="efficiency.png" style="zoom:100%" />

印象笔记（Evernote）是一款很好用的跨平台记笔记软件。

Alfred 是mac上的一款高效搜索软件。

Teamviewer 是远程控制桌面的软件，团队协作必备。

不想用百度云的我于是使用了 Dropbox，除了空间比较小，还需要翻墙外，别的方面都挺好的。

<!-- ![efficiency2](/images/我的MacBook/efficiency2.png) -->

<img src="efficiency2.png" style="zoom:40%" />


mpv 是一款开源的播放器，占用内存小，运行速度快。

Navicat Premium 是我用过的最好用的MySQL数据库工具，比 MYSQL workbench 不知道高到哪里去了。





