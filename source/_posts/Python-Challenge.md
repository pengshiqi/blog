---
title: Python Challenge
date: 2016-08-27 13:57:44
tags:
---

最近发现了一个有趣的闯关游戏，叫[Python Challenge](http://www.pythonchallenge.com/)。在做题的同时，也能学习到一些新的python模块。

我的解题代码放在[我的github上](https://github.com/pengshiqi/PythonChallenge).
<!-- more -->

### Level 0

将2的38次方替换掉URL中的0.

### Level 1

字符整体后移两位，原本URL中的map后移两位便是 *OCR*。

### Level 2

查看网页源代码，一串乱序的字符串中，夹杂着有用的信息，提取出其中的字母，得到 *equality*.

### Level 3

还是在网页源代码的注释中，找到 *恰好* 三个大写字母从两侧包围一个小写字母的情况，找到单词 *linkedlist*。

### Level 4

点击图片看到 *and the next nothing is 44827*，将URL的12345替换为44827后，得到45439，以此类推，很多次之后得到单词 *peak*.

### Level 5

和 peak hell 发音很像的是 pickle,说明这题要用到python的pickle库。从banner.p处拿到文件，用pickle处理得到单词 *channel*。

### Level 6

图片是拉链 zip，而 zip 又有压缩文件的意思，所以讲URL的后缀改为zip得到 channel.zip 的压缩文件。

解压压缩文件后，从 readme 可知，又要不停地寻找 the next nothing，最后得到：collect the comments.

原来，zip文件有comments属性，python 有 zipfile 库来处理。

获得每个中间文件的comments，得到最后的单词 *oxygen*。

### Level 7

图片中央有一条线，通过python的Image库来分析这条线，取这条线上的点的像素，并将其转换成ascii码，得到单词 *integrity*。

### Level 8

python的bz2模块。

在网页源代码中有加密后的username和password，通过bz2.decompress()函数能解密之。输入 huge 和 file 即可跳转到 good.html。

### Level 9

first 和 second 连接起来，相邻的两个数字作为一个点的坐标，画出图形，是一头牛，于是答案便是 *bull*。

### Level 10

找规律，第零个数是1，第一个数是11，表示1个1，第二个数是21，表示2个1，第三个数是1211，表示1个2、1个1，一次类推到第30个数，第三十个数的长度为 *5808*，即为答案。

### Level 11

图像的名字叫even odd，暗示我们要把这张图的像素点分奇偶提取出来，分别提取出坐标为奇数和偶数的像素，作图可得单词 *evil*。

### Level 12

先找到 evil2.gfx 文件，原图里那个人把扑克牌分成五堆，所以这里把这个gfx分成五个文件，得到单词 *disproportional*。

### Level 13

按下数字键5，会进入一个xml界面，这个界面用了RPC(Remote Procedure Call)协议，通过python的xmlrpclib库可以处理它，向 Bert 打电话，可得单词 *ITALY*。

### Level 14

下方那张图片实际上是 10000*1 的，标题叫walk around，而且上方的图片是螺旋状的面包，暗示着我们要把下面的图片按照螺旋状排开，得到了一只猫的图片，所以答案是 *cat*。

### Level 15

从右下角可以看出这年是闰年，而且是1**6年，通过datetime模块来判断符合要求的年份，得到如下几个：

* 1176-01-26
* 1356-01-26
* 1576-01-26
* 1756-01-26
* 1976-01-26

而又说到是第二年轻的，所以是1756年。第二天要买花，查一查1756-01-27，发现是莫扎特的诞辰，所以答案是 *Mozart*。

### Level 16

标题说 let me get this staight，于是我们就按照每行第一个粉红色的点来对齐，得到单词 *romance*。

### Level 17

这一关有点麻烦，首先获取网页的cookie提示，知道要回到第四关linkedlist，用busynothing替换掉之前的nothing，而且这次要收集每次的cookie信息。然后利用之前的bz2解码方式对这些信息解码，得到 *‘is it the 26th already? call his father and inform him that "the flowers are on their way". he'll understand.’*。然后像13题一样，给Mozart的父亲打电话，得到 *555-VIOLIN*。将 ‘the flowers are on their way’ 作为cookie传入这个页面，得到最终的提示 *‘oh well, don't you dare to forget the balloons.’*。所以答案就是 *balloons*。

### Level 18

提示说两幅图的区别，显然就是brightness。然后下载deltas.gz的文件。这个压缩文件里是两片16进制的数据，利用difflib库的ndiff函数来找出它们的不同(缺点就是计算的好慢啊。。)，结果是三幅图片，分别是 '/hex/bin.html', 'butter', 'fly'，所以在bin.html中输入username和password即可过关。

### Level 19

将网页源代码中的文本信息转换成音频wav文件，encoding方式为base64，然后发现只有一个单词 sorry，看提示图片可知，需要把每一帧音频反转，得到的音频文件内容为 ‘You are an idiot'，于是，答案便为 *idiot*。

### To be continued...
