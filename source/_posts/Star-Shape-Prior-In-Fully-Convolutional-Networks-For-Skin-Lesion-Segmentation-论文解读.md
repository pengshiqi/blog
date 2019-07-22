---
title: Star Shape Prior in Fully Convolutional Networks for Skin Lesion Segmentation 论文解读
date: 2019-03-03 19:30:08
tags: [Medical, Image Segmentation]
categories: [Paper Report]
---

这是发表在 MICCAI 2018 上的一篇关于病灶图像分割的文章。

arxiv链接在[https://arxiv.org/abs/1806.08437](https://arxiv.org/abs/1806.08437)。

<!-- more -->

-----

## Motivations

深度卷积网络已经是做像素级分类预测任务的首要手段，但是目前还没有一个很好的方法去将人类的先验知识融入到模型中。

作者的灵感来源是之前在能量方程最小化方面的工作（ASM，graph-based methods...），于是在这篇文章中，将Veksler star shape prior设计成了损失函数，来惩罚non-star shape的FCN预测，以保证分割结果的形状。作者提出的方法在ISBI 2017皮肤分割数据集上排名 1/21。



## Methodology

**Star Shape Regularized Loss**

Star shape的定义是这样的：

> Assuming c is the center of object O, object O is a star shape object if, for any point p interior to the object, all the pixels q lying on the straight line segment connecting p to the object center c are inside the object.

如下图(a)所示。

<img src="image1.png" style="zoom:90%" />

然后基于star shape的定义，作者设计了这样一个损失函数：

<img src="image2.png" style="zoom:80%" />

其中，$y_{ip}$是在图像$i$中像素$p$的真实标签。

这个loss要求(1)如果p和q有相同的gt标签且(2)p被分类错误，那么p和q应该有相同的预测标签。



## Experiments

作者基于两个FCN进行了实验：（1）U-net；（2）ResNet-DUC (Dense Upsampling Convolution)。

实验数据集是ISBI 2017，2000(train) + 150(val) + 600(test)，其中仅有0.14%的图片不是star-shaped。

下表是实验结果：

（所有方法的SP都很高，但这篇文章的方法把SE提高了近5个点，Dice和Jaccard提高了1%。）

<img src="image3.png" style="zoom:90%" />

下图展示的是有无Star shape约束时的效果对比。

<img src="image4.png" style="zoom:90%" />



## Conclusion

这篇文章的主要内容是，受到传统的能量最小化方法的启发，将对目标分割区域的形状先验知识通过约束条件添加进深度网络模型，让模型的分割结果呈现出预期的状态。

这种思路我们可以思考借鉴。