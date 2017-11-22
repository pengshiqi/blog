---
title: Python中的虚拟环境
date: 2016-10-03 17:11:49
tags: Python
categories: Python
---

不同的项目之间可能会使用不同版本的某些包，但是因为某些原因（比如有依赖冲突）却不能都升级到最新版本。这时候就需要对环境进行隔离，使用虚拟环境让全局的site-packages目录非常干净和可管理。

<!-- more -->

这里我们使用virtualenv来创建管理单独、干净的python环境。

### virtualenv

先安装virtualenv：
```bash
sudo pip install virtualenv
```

现在创建一个Python环境：
![icon](http://obw22u9v2.bkt.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202016-10-03%20%E4%B8%8B%E5%8D%885.25.16.png)

virtualenv默认会创建一个包含了Python可执行文件、常用的标准库、激活virtualenv环境的脚本的目录。

使用source激活virtualenv环境：
![icon](http://obw22u9v2.bkt.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202016-10-03%20%E4%B8%8B%E5%8D%885.29.19.png)

注意终端提示的改变，前面添加了'(venv)'前缀。

可以看到已经不再使用系统环境变量中的Python了。如果要退出虚拟环境，可以取消激活：
```bash
(venv)> deactivate
```
