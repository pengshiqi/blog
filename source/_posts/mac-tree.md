---
title: Unix系统下的tree命令——展示目录树结构
date: 2016-08-17 20:05:15
categories:
- 学习
tags: 
- Mac
- Linux
---

在UNIX系统下，我们可以很方便地使用tree命令来查看当前目录下的目录树结构。

### Ubuntu 15.10

在Ubuntu的命令行中，首先安装tree命令：
<!-- more -->
```
sudo apt-get install tree
```
<!-- ![icon](http://obw22u9v2.bkt.clouddn.com/linux1.png) -->
<img src="http://obw22u9v2.bkt.clouddn.com/linux1.png" style="zoom:100%" />

然后用tree命令查看当前目录下某一文件夹的结构：
```
tree DIRECTORY_NAME
```
<!-- ![icon](http://obw22u9v2.bkt.clouddn.com/linux2.png) -->
<img src="http://obw22u9v2.bkt.clouddn.com/linux2.png" style="zoom:100%" />


### OS X 10.11.6

在Mac terminal中用brew安装tree：
```
sudo brew install tree
```
<!-- ![icon](http://obw22u9v2.bkt.clouddn.com/mac1.png) -->
<img src="http://obw22u9v2.bkt.clouddn.com/mac1.png" style="zoom:100%" />


然后用tree命令查看当前目录下某一文件夹的结构：
```
tree DIRECTORY_NAME
```
<!-- ![icon](http://obw22u9v2.bkt.clouddn.com/mac2.png) -->
<img src="http://obw22u9v2.bkt.clouddn.com/mac2.png" style="zoom:100%" />

